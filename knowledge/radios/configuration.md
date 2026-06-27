# Radio Configuration

## Summary

The radio configuration defines the LoRa parameters used by the SX1262/SX1268 radios. The defaults are set in code, but the tuned center frequency can be changed at runtime within the selected band.

## Band Selection

The radio hat has a band pin. On startup, the ground station radio reads this pin to decide which band and chip family it is working with.

| Band | Default Center Frequency | Radio IC | Code Band Name | Allowed Runtime Frequency Range |
| --- | ---: | --- | --- | --- |
| 433 MHz band | 435.00 MHz | SX1268 | B435 / SystemA | Greater than 430 MHz and less than 440 MHz. |
| 900 MHz band | 903.00 MHz | SX1262 | B903 / SystemB | Greater than 902 MHz and less than 928 MHz. |

The exact center frequency is not fixed forever. It can be changed during operations, as long as it stays inside the selected band and complies with LC/radio-operator constraints. LC frequency approval is expected to happen ad-hoc during competition, so runtime adjustability is the main requirement.

## Default LoRa Parameters

Current defaults from radios-2026:

| Setting | Value | Notes |
| --- | --- | --- |
| Bandwidth | 250 kHz | RadioLib parameter. |
| Spreading factor | 8 | RadioLib parameter. |
| Coding rate denominator | 8 | Corresponds to coding rate 4/8 in project shorthand. |
| Sync word | 0x12 | Used to filter unrelated LoRa transmissions. |
| Software TX power setting | 22 dBm | RadioLib/SX126x setting in code. |
| Effective boosted TX power | About 30 dBm | Due to Dorji SX126x shield/amplifier path. |
| Preamble length | 12 | RadioLib parameter. |
| TCXO voltage | 3.0 V | Board/radio configuration. |
| Radio buffer size | 256 bytes | Software buffer limit. |

## Runtime Changes

Radio commands can change parameters such as frequency, bandwidth, spreading factor, coding rate, and power. Frequency changes are checked against the current band, so a 433 MHz radio cannot be set to a 900 MHz frequency and vice versa.

## Source Code Context

- radios-2026/GroundStation/include/configs/LoraParamConfig.h
- radios-2026/GroundStation/src/datastore/ParamStore.cpp
- radios-2026/GroundStation/include/datastore/ParamStore.h

## Related Pages

- [LoRa radio link](lora-link.md)
- [Radio startup behavior](radio-startup-behavior.md)
- [Launch Canada requirement notes](../context/launch-canada-requirement-notes.md)
- [Radio command interface](radio-command-interface.md)

## Open Questions

- Should any non-default LoRa settings be used for LC, or are the current defaults final?
