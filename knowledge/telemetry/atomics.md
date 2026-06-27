# Telemetry Atomics

## Summary

An atomic is a named group of telemetry fields that can be included in an ASTRA frame payload. The atomics bitmap in the ASTRA header tells the receiver which atomic groups are present.

Atomics let ASTRA send only selected groups of telemetry fields while keeping decoding deterministic. They also support packet-size optimization because the flight computer can send different atomic groups in different flight stages.

## Field and Atomic Terminology

| Term | Meaning |
| --- | --- |
| Field | One specific telemetry value from the flight computer, such as altimeter altitude or main oxidizer valve state. |
| Type | The C++-compatible data type for a field, such as int16, float, or boolean. |
| Atomic | A fixed group of fields defined together in the telemetry sheet and generated into C++ structs. |
| Atomics bitmap | 32-bit ASTRA header field where bit i means atomic i is present in the payload. |

## How Atomics Are Defined

The H26 AV packet coordinator Google Sheet defines the fields, their types, and how they are grouped into atomics. The automation script generates C++ definitions from the sheet.

The FC, ground station radios, and ground station control code must all share the same atomic definitions. If they agree on the definitions, they can reinterpret the variable payload bytes back into the correct fields.

## Payload Ordering Rule

Atomic groups are serialized in increasing atomic index order.

Example: if the bitmap is binary 101, atomic 0 and atomic 2 are present. The payload contains atomic 0 first, then atomic 2. Atomic 1 is skipped.

This ordering rule is what lets the receiver calculate offsets into the variable payload.

## Current Generated Atomic Catalog

The inspected telemetry-2026 generated code currently defines these atomic indices:

| Index | Atomic |
| ---: | --- |
| 0 | recov_atomic |
| 1 | prop_states_atomic |
| 2 | prop_atomic |
| 3 | flight_stage_atomic |
| 4 | fc_internal_atomic |
| 5 | altitude_atomic |
| 6 | altitude_events_atomic |
| 7 | acceleration_atomic |
| 8 | gyro_atomic |
| 9 | gps_atomic |
| 10 | radio_atomic |
| 11 | sd_atomic |
| 12 | payload_atomic |

## Calibration and Scheduling

Some fields are encoded in compact raw types and converted later by GSC calibration metadata. The atomics included in ASTRA packets can also change by flight stage to reduce telemetry load.

- [GSC telemetry calibration](../ground-station/gsc-telemetry-calibration.md)
- [Flight-stage atomic scheduling](flight-stage-atomic-scheduling.md)

## Source Artifacts

Known local resources related to atomics:

- ExternalResources/AV ASTRA google sheet/Atomics.html
- ExternalResources/AV ASTRA google sheet/Full Packet.html
- ExternalResources/AV ASTRA google sheet/Atomic Frequency.html
- ExternalResources/AV ASTRA google sheet/Encoding Types.html
- ExternalResources/AV ASTRA google sheet/Parameters.html
- telemetry-2026/parser/parse_sheet.py
- telemetry-2026/telemetry/gen/telemetry_packets.h
- telemetry-2026/telemetry/gen/telemetry_packets.cpp

## Related Pages

- [Telemetry overview](overview.md)
- [ASTRA frame format](../protocols/astra-frame-format.md)
- [ASTRA protocol](../protocols/astra.md)

## Open Questions

- What is the authoritative latest version of the H26 AV packet coordinator sheet?
- Which fields are most important for the ground station GUI?
- Are atomic definitions frozen for Launch Canada 2026, or still changing?
- What compatibility rule should be used if FC, GS radios, and GSC are built from different generated versions?
