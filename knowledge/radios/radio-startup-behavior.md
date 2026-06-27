# Radio Startup Behavior

## Summary

On startup, each ground station radio determines its band, initializes the LoRa radio with default parameters, starts Ethernet/MQTT connectivity, publishes status for GSC, and enters receive mode.

## Startup Sequence

1. Read the radio hat band pin.
2. Select the band and default radio parameters.
3. Initialize the SX1262/SX1268 radio through RadioLib.
4. Configure the radio with the default frequency, bandwidth, spreading factor, coding rate, sync word, power, preamble, TCXO voltage, and current-limit settings.
5. Initialize Ethernet on the Teensy 4.1.
6. Check for physical Ethernet link.
7. Use DHCP to get an IP address from the router.
8. Resolve the MQTT broker hostname mrt_mqtt through router DNS.
9. Connect to the MQTT broker on port 1883.
10. Publish retained status/detail topics so GSC can detect the radio as alive.
11. Subscribe to rocket-command and radio-command MQTT topics.
12. Enter receive mode.

## Band Detection

The radio hat band pin determines whether the radio starts as a 433 MHz-band radio or a 900 MHz-band radio. Once the band is selected, the actual center frequency can still change within that band.

## Network Startup

The radio first needs Ethernet link, then DHCP, then DNS, then MQTT. If Ethernet is unplugged, the radio waits rather than trying to complete the MQTT path.

The router maps mrt_mqtt to the computer running the MQTT broker, usually the main ground station computer.

## Alive/Dead Detection

The radio publishes retained MQTT status so GSC can detect that the radio is alive. Current retained status behavior:

| Radio State | Retained Status |
| --- | --- |
| TX-from-CTS enabled | OK |
| TX-from-CTS disabled | RECEIVE |
| Failure reported by code | FAILED |

The MQTT connection uses a last-will path to clear or terminate the retained status topic if the radio disconnects or dies, allowing GSC to detect that it is no longer connected.

## Receive Mode Default

Normal ground station radios start receive-only. TX-from-CTS is disabled by default for pad and control-station variants and must be enabled intentionally from GSC via radio command.

## Source Code Context

- radios-2026/GroundStation/src/datastore/ParamStore.cpp
- radios-2026/GroundStation/src/radios/RadioModule.cpp
- radios-2026/GroundStation/src/core/ConsoleRouter_eth.cpp
- radios-2026/GroundStation/src/variant/GroundStationVariant.cpp

## Related Pages

- [Radio configuration](configuration.md)
- [Ground station radio network and MQTT](../ground-station/radio-network-and-mqtt.md)
- [Radio command interface](radio-command-interface.md)

## Open Questions

- What exact timeout/status should GSC show for a radio that stops refreshing MQTT status?
- Is the retained empty last-will behavior final, or should a specific DISABLED/UNAVAIL payload be used?
- What user-facing names should GSC show for each pad/control-station radio?
