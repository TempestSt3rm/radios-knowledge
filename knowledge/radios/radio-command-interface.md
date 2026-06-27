# Radio Command Interface

## Summary

Radio commands are GSC-to-radio commands used to control or inspect the ground station radio itself. They are separate from rocket commands, which are queued and sent over RF to the flight computer after CTS.

For launch operations, the most important radio command is enabling or disabling TX-from-CTS.

## Command Path

1. GSC publishes a radio command over MQTT to the radio command topic for a specific radio.
2. The radio receives the command through MQTT.
3. The radio parses it as a ground/radio command.
4. If valid, the radio applies the change and publishes an acknowledgement/status update.

## TX-from-CTS

TX-from-CTS controls whether a ground station radio is allowed to transmit a queued rocket command when it receives CTS from the flight computer.

| State | Behavior |
| --- | --- |
| Disabled | Radio receives telemetry and ignores rocket commands for RF transmission. |
| Enabled | Radio may queue rocket commands and transmit one after CTS. |

Pad and control-station radios start with TX-from-CTS disabled. GSC enables it through a radio command when that physical radio should be allowed to transmit.

## Other Radio Commands

The code also supports radio commands for configuration and debugging, including:

| Command Area | Purpose |
| --- | --- |
| Frequency | Change tuned center frequency within the selected band. |
| Bandwidth | Change LoRa bandwidth. |
| Spreading factor | Change LoRa spreading factor. |
| Coding rate | Change LoRa coding rate. |
| Power | Change software TX power setting. |
| Parameter check / ping | Report current radio configuration. |
| Status | Set or refresh status visible to GSC. |
| Clear | Clear queued rocket commands. |
| Verbose/logging | Change debug print/log categories. |

These are useful for testing and operations, but TX-from-CTS is the key command for controlling RF command authority.

## Source Code Context

- radios-2026/GroundStation/src/commands/GroundCommand.cpp
- radios-2026/GroundStation/src/core/Groundstation.cpp
- radios-2026/GroundStation/src/core/ConsoleRouter_eth.cpp

## Related Pages

- [Radio startup behavior](radio-startup-behavior.md)
- [Ground station radio network and MQTT](../ground-station/radio-network-and-mqtt.md)
- [Radio configuration](configuration.md)

## Open Questions

- What exact GUI labels/buttons map to TX-from-CTS enable/disable?
- Should GSC prevent enabling TX-from-CTS on more than one radio per band at once?
- Which radio commands are considered launch-operator controls versus developer/debug controls?
