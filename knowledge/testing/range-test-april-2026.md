# April 2026 Range Test

## Summary

The April 2026 range test provides evidence that the radio system can operate at least 5 km with multiple antenna setups. The results are especially relevant to antenna selection, link margin, and confidence in Launch Canada 2026 radio performance.

## Test Context

| Item | Value |
| --- | --- |
| Range | 5 km |
| Radio hat | Radio Hat V3 designed by Max Zischka |
| Teensy breakout | 4.1 Breakout V2 by TingYi Chen |
| FC-side antenna setup | Small 433 MHz and 900 MHz commercial whips were used on the FC side. |
| Ground-station antenna setup | The GS side changed antennas to compare performance. |
| Personnel | Jeffrey Lim, Max Zischka, TingYi Chen, Mathew Mora, Martin Laberte, Micha Reneault |

## Metrics

| Metric | Meaning |
| --- | --- |
| GS RSSI average | Average received signal strength of valid packets received by the ground station. |
| GS SNR average | Average signal-to-noise ratio of valid packets received by the ground station. |
| FC RSSI average | Average received signal strength of valid packets received by the FC side. |
| FC SNR average | Average signal-to-noise ratio of valid packets received by the FC side. |
| Telemetry success rate | Packet reception percentage based on sequence numbers. 100% means no sequence number was dropped during the test. |
| Command success rate | Percentage of command packets that made it through. In the test logs, this is the likelihood that a CTS row is followed by an ACK packet. |

## Summary Results

| Antenna Tested | GS RSSI Avg | GS SNR Avg | FC RSSI Avg | FC SNR Avg | Runtime | Telemetry Success | Command Success |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| Yagi 433 | -77.9 | 11.6 | -75.3 | 10.9 | 67 s | 99.6% | 99.2% |
| CWhip 433 | -75.9 | 11.8 | -74.7 | 12.1 | 67 s | 100.0% | 99.2% |
| Whip 433 | -79.4 | 11.7 | -76.4 | 11.8 | 67 s | 100.0% | 99.2% |
| Omni 433 | -91.3 | -5.5 | -87.0 | 7.0 | 85 s | 78.0% | 97.1% |
| Patch 433 (SRAD) | -101.5 | 8.6 | -97.8 | -1.0 | 73 s | 100.0% | 98.5% |
| Dipole 433 (SRAD) | -105.2 | 5.2 | -102.5 | -6.1 | 67 s | 100.0% | 84.2% |
| Yagi 900 | -90.5 | 6.3 | -85.4 | 5.2 | 72 s | 93.9% | 93.6% |
| CWhip 900, good position | -91.8 | 1.8 | -88.9 | 3.3 | 63 s | 98.0% | 95.1% |
| Whip 900 | -96.5 | 5.9 | -91.0 | 1.3 | 98 s | 95.1% | 91.4% |
| Omni 900 | -99.7 | -0.6 | -92.9 | -0.6 | 70 s | 94.1% | 84.4% |
| Patch 900 (SRAD) | -115.0 | -4.9 | -105.3 | -11.7 | 82 s | 87.4% | 14.8% |
| CWhip 900, bad position | -93.3 | -0.6 | -89.7 | 2.4 | 79 s | 80.8% | 88.2% |

## Notes

- The test data is more directly related to antennas than to ASTRA itself, but it supports confidence in the radio link.
- The CWhip 900 result depended strongly on physical position. Moving it a few inches dramatically improved performance in the recorded data.
- The raw logs include sequence number, action, GS time, GS RSSI, GS SNR, FC time, FC RSSI, FC SNR, and error rows.
- Error rows include CRC errors and missing sequence numbers, which indicate packet corruption or complete packet loss.

## Source Artifacts

- ExternalResources/RangeTest April 2026/Data Summary.html
- ExternalResources/RangeTest April 2026/Explainer.html
- ExternalResources/RangeTest April 2026/Metadata.html

## Related Pages

- [LoRa radio link](../radios/lora-link.md)
- [Flight-stage atomic scheduling](../telemetry/flight-stage-atomic-scheduling.md)

## Open Questions

- What exact date in April 2026 was the range test performed?
- Which antenna setup should be considered the baseline for Launch Canada 2026?
- Were both 433 MHz and 900 MHz tested with the same LoRa settings as flight configuration?
- What were the exact center frequencies used during the test?
- Should the raw range test logs be added to this repository or only the summary exports?
