# Ground Station Radio Network and MQTT

## Summary

The ground station radio network connects the ground station computer, pad radios, control-station radios, and other embedded devices over one local area network. MQTT is used as the pub-sub layer between embedded radios and Ground Station Control (GSC).

The key design idea is decoupling: the radio firmware can publish telemetry and receive commands without directly knowing how the GUI is implemented.

## Physical Radio Layout

Each band has three radios in the full system:

| Location | Band A / 433 MHz | Band B / 900 MHz | Purpose |
| --- | --- | --- | --- |
| Rocket / FC | One rocket-side radio | One rocket-side radio | Transmits telemetry and receives commands. |
| Control station | One ground radio | One ground radio | Receives telemetry near the human operators and can transmit commands if enabled. |
| Pad | One ground radio | One ground radio | Receives telemetry at the pad and can transmit commands if enabled. |

This means there are four ground station radios total: two bands times two ground locations. The two physical ground locations provide more receive opportunities and let commands be sent from a selected enabled radio.

## Network Topology

The control station and pad are connected into one local area network. A Wi-Fi antenna link connects the pad and control station sides.

The router is a GL.iNet travel router. The pad and control station each have network switches. These switches provide Ethernet and Power over Ethernet (PoE). The ground station radios use Teensy 4.1 boards because Teensy 4.1 supports Ethernet. A PoE splitter can provide both data and power to the Teensy/radio assembly. The switches eventually connect to the router, and the ground station computer also connects to this router.

## MQTT Broker Discovery

The ground station computer runs the MQTT broker. Usually this is the main ground station computer, but during testing it can be another computer.

The router provides DNS. It maps the hostname mrt_mqtt to the IP address of whichever computer is running the MQTT broker.

Embedded devices resolve mrt_mqtt through the router, then connect to the MQTT broker on port 1883.

## MQTT Purpose

MQTT is a publish-subscribe server. Topics are string channels used to group messages logically.

The radios use MQTT to:

- Publish raw rocket telemetry frames to GSC.
- Publish matching radio metadata for link-quality debugging.
- Publish radio-side telemetry and status.
- Receive rocket commands from GSC.
- Receive radio configuration commands from GSC.
- Publish command reception/transmission acknowledgements back to GSC.

## Rocket Command Routing

GSC publishes rocket commands to the flight-computer command topic for the relevant band. Both ground radios for that band can receive the same command topic.

A radio only queues and later transmits the command if TX-from-CTS is enabled. If TX-from-CTS is disabled, the radio ignores rocket commands for RF transmission. TX-from-CTS is controlled from the GUI through radio commands.

This allows GSC to publish commands without caring which physical radio will transmit. The transmit authority is controlled inside each radio.

## TX-from-CTS Setting

The radio-side setting is called canTxFromCTS in the current code. Conceptually, it means Enable TX from CTS.

| Setting | Meaning |
| --- | --- |
| Disabled | The radio stays receive-only for rocket commands. It does not queue rocket commands for CTS transmission. |
| Enabled | The radio may queue rocket commands and transmit one when CTS arrives. |

Current radios-2026 ground-station variants initialize pad and control-station radios with TX-from-CTS disabled. It must be enabled intentionally. A no-Ethernet testing variant enables it by default.

## Topic Names from radios-2026

Current code defines topics in GroundStation/include/configs/MqttTopics.h.

| Purpose | Band A Topic | Band B Topic |
| --- | --- | --- |
| Rocket telemetry | SystemA/Rocket/FlightComputer/telemetry | SystemB/Rocket/FlightComputer/telemetry |
| Rocket commands | SystemA/Rocket/FlightComputer/commands | SystemB/Rocket/FlightComputer/commands |
| Control-station radio ACKs | SystemA/ControlStation/Radio/acks | SystemB/ControlStation/Radio/acks |
| Control-station radio telemetry | SystemA/ControlStation/Radio/telemetry | SystemB/ControlStation/Radio/telemetry |
| Control-station radio commands | SystemA/ControlStation/Radio/commands | SystemB/ControlStation/Radio/commands |
| Pad radio ACKs | SystemA/Pad/Radio/acks | SystemB/Pad/Radio/acks |
| Pad radio telemetry | SystemA/Pad/Radio/telemetry | SystemB/Pad/Radio/telemetry |
| Pad radio commands | SystemA/Pad/Radio/commands | SystemB/Pad/Radio/commands |

The topic table also includes status, detail, debug, and name topics for each radio role/band.

## Status Topics

The current topic comments describe radio status values such as OK, RECEIVE, FAILED, UNAVAIL, and DISABLED.

The current code publishes retained status:

| TX-from-CTS | Retained Status |
| --- | --- |
| Enabled | OK |
| Disabled | RECEIVE |

## Code Notes from radios-2026

Observed implementation details:

- Broker hostname: mrt_mqtt.
- MQTT port: 1883.
- Topic table: GroundStation/include/configs/MqttTopics.h.
- MQTT routing and subscriptions: GroundStation/src/core/ConsoleRouter_eth.cpp.
- Rocket command queueing: GroundStation/src/commands/CommandParser.cpp.
- TX-from-CTS state: GroundStationStore::canTxFromCTS.
- Radio command keyword: radio.

The Pad Band B ACK topic typo previously observed in MqttTopics.h has been resolved; Pad Band B ACKs now use SystemB/Pad/Radio/acks.

## Related Pages

- [ASTRA runtime behavior](../protocols/astra-runtime-behavior.md)
- [ASTRA command packets](../protocols/astra-command-packets.md)
- [GSC telemetry calibration](gsc-telemetry-calibration.md)
- [Raw telemetry and metadata over MQTT](raw-telemetry-and-metadata-mqtt.md)
- [Radio startup behavior](../radios/radio-startup-behavior.md)
- [Radio command interface](../radios/radio-command-interface.md)
- [Radio hat hardware](../radios/radio-hat-hardware.md)

## Open Questions

- Can more than one radio per band ever have TX-from-CTS enabled at the same time, or must exactly one be enabled?
- What are the final MQTT topics expected by GSC, and are they frozen for Launch Canada 2026?
- What exact Wi-Fi antenna/bridge hardware is used between pad and control station?
- What GL.iNet router model and DNS/DHCP configuration are used?
