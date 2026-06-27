# LoRa Radio Link

## Summary

The radio system uses Semtech SX1262 and SX1268 LoRa radio ICs. LoRa is used because it is optimized for long range, low power, and strong noise resistance.

This page focuses on the project-relevant LoRa facts. The detailed RF theory of LoRa can be found in external references and is not the main purpose of this knowledge base.

## LoRa Basics

LoRa uses chirp spread spectrum modulation. In simple terms, information is encoded through changes in frequency over a bandwidth. The radio ICs handle modulation, demodulation, error correction, and packet integrity checks.

For the project, the important practical point is that software can pass an array of bytes to the radio stack, and the radio IC handles the lower-level RF transmission details.

## Signal-to-Noise Ratio

Signal-to-noise ratio (SNR) compares the strength of the desired signal to the strength of background noise.

- Positive SNR means the signal is stronger than the noise.
- Negative SNR means the signal is weaker than the noise.
- LoRa can often decode packets even with negative SNR, which is one reason it is useful for long-range noisy links.

## Radio ICs and Bands

| Flight Computer | Band | Radio IC | Purpose |
| --- | --- | --- | --- |
| FC A | 433 MHz | SX1268 | Redundant telemetry and command path. |
| FC B | 900 MHz | SX1262 | Redundant telemetry and command path. |

The two flight computers operate on different bands to provide redundancy for both telemetry and commands.

## Current Radio Configuration

The current default LoRa settings are documented in [radio configuration](configuration.md). Center frequencies can be changed at runtime within the selected band, which supports LC frequency-agility expectations.

## Hardware Note

The radio ICs are from Semtech. The project bought Dorji SX126x shields and mounted them to custom radio hat PCBs. The radio hats are used by both the flight computer side and the ground station side.

## Test Evidence

The April 2026 range test recorded radio performance at 5 km across multiple 433 MHz and 900 MHz antenna setups. See [April 2026 range test](../testing/range-test-april-2026.md).

## Related Pages

- [ASTRA protocol](../protocols/astra.md)
- [Radio hat hardware](radio-hat-hardware.md)
- [Radio configuration](configuration.md)
- [Radio startup behavior](radio-startup-behavior.md)
- [Antennas](antennas.md)
- [System overview](../context/system-overview.md)

## Open Questions

- Are CRC setting, header mode, and packet length mode explicitly configured or left at RadioLib defaults?
- Are both bands always active during launch operations?
