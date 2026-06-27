# LC 2026 Telemetry, Radios, and Ground Station Overview

## Summary

The Launch Canada 2026 radio system moves telemetry from the rocket to the ground station and sends commands from the ground station back to the rocket. Telemetry supports live operator awareness through the ground station GUI, while commands support ground-to-rocket actions such as launch-related commands.

## Main Data Flow

1. The flight computer (FC) generates telemetry bytes.
2. The FC team also saves telemetry to the onboard SD card.
3. The radio-side software formats telemetry bytes into an ASTRA frame.
4. The radio driver sends the ASTRA frame to the radio hat PCB.
5. The radio hat uses a Semtech SX1262 or SX1268 LoRa radio IC to transmit the frame over RF.
6. The ground station radio receives the LoRa packet.
7. The ground station radio interprets the received ASTRA frame and sends the data over LAN to the ground station computer.
8. The ground station GUI displays the live rocket state for operators.

## Reverse Command Flow

Commands travel from the ground station to the rocket. One example command is LAUNCH.

Because the radios are half-duplex, only one side should transmit on the RF link at a time. This is a major reason the project needs a telemetry protocol layer. In ASTRA, the FC is the leader: it sends telemetry and periodically opens command opportunities with CTS. Ground station radios follow by staying in receive mode unless CTS arrives and TX-from-CTS is enabled.

## Ownership Boundaries

| Area | Notes |
| --- | --- |
| Telemetry generation | Handled by the flight computer team. |
| Onboard SD logging | Handled by the flight computer team. |
| ASTRA frame formatting | Radio/protocol-side responsibility. |
| Radio drivers | Partially written by the radio-side contributor. Drivers differ between FC and ground station hardware. |
| Ground station GUI | Managed by the Ground Station Control team. |
| Ground station radio hardware/software | Ground station radio-side area of expertise. |

## Terms

| Term | Meaning |
| --- | --- |
| FC | Flight computer. The rocket-side computer that generates telemetry. |
| Rocket | In this knowledge base, often shorthand for the rocket-side FC and radio stack. |
| Ground station | The receive/transmit station on the ground, including radios, LAN forwarding, computer, and GUI. |
| Telemetry | Data about the rocket state sent from the rocket to the ground. |
| Command | A message sent from the ground to the rocket. |
| Half-duplex | Radio constraint where the link supports communication in both directions, but not at the same time. In this project, the rocket and ground station must coordinate who transmits. |
| ASTRA frame | Rocket-to-ground telemetry structure made of an 8-byte header and a variable-length atomic payload. |
| Atomic | Fixed group of telemetry fields that can be included in an ASTRA payload. |
| Command ID | Ground-generated identifier used to match a command to its acknowledgement. |
| GSC | Ground Station Control. The GUI/control software that consumes telemetry and publishes operator commands. |
| MQTT | Pub-sub messaging system used on the ground station LAN to move telemetry, commands, status, and radio messages. |

## Related Pages

- [Telemetry overview](../telemetry/overview.md)
- [ASTRA protocol](../protocols/astra.md)
- [ASTRA frame format](../protocols/astra-frame-format.md)
- [ASTRA command packets](../protocols/astra-command-packets.md)
- [ASTRA runtime behavior](../protocols/astra-runtime-behavior.md)
- [Ground station radio network and MQTT](../ground-station/radio-network-and-mqtt.md)
- [LoRa radio link](../radios/lora-link.md)
- [Radio hat hardware](../radios/radio-hat-hardware.md)

## Open Questions

- What are the exact launch-day command types besides LAUNCH?
- What are the exact operator roles that interact with the telemetry GUI and command path?
