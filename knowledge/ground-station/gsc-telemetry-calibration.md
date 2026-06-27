# GSC Telemetry Calibration

## Summary

Some ASTRA telemetry fields are encoded for packet efficiency rather than direct human readability. The Ground Station Control (GSC) side applies calibrations and units so operators see meaningful values in the GUI.

This matters because the radio layer may see raw packed values, while the GUI should show calibrated engineering values.

## Why Calibration Exists

ASTRA tries to reduce packet size where possible. Smaller packets reduce transmit latency and reduce the amount of RF data that can be corrupted or lost.

Example: if a value only ranges from -40 to 85 and needs 0.01 precision, it does not need to be sent as a 32-bit float. It can be packed into a smaller integer representation, then converted back to a normal display value later.

## Responsibility Split

| Area | Responsibility |
| --- | --- |
| H26 AV packet coordinator sheet | Defines field encoding, GUI type, display units, and calibration function. |
| FC side | Converts the real value into the chosen encoded representation before packing it into an atomic. |
| Radio layer | Treats the bytes as payload. It normally does not care about calibration unless someone is manually inspecting raw data. |
| GSC side | Uses generated XML/calibration metadata to convert raw values into display values for the GUI. |
| GUI operators | See the calibrated value and useful units, not the space-efficient raw representation. |

## Sheet Columns Involved

The Parameters sheet includes columns for raw units, display units, GUI type, encoding, calibration function, and metadata. The calibration function is what lets GSC convert from the encoded value to the GUI-facing value.

Readable examples from the sheet export:

| Packet Variable | Display Units | GUI Type | Encoding | Calibration Function | Notes |
| --- | --- | --- | --- | --- | --- |
| tank_pressure | psi | Float | uint16 | x 469.22 - 2.4813 / | Based on real calibration test. |
| tank_temp_celsius | C | Float | int16 | x 100 - | Temperature packed as integer then scaled. |
| vent_temp_celsius | C | Float | int16 | x 10 / | Sheet note says this maps an integer into a larger float range. |
| mov_hall_state | Unknown | Boolean | boolean | None listed | Boolean state. |

## Important Radio-Side Note

For normal radio operation, calibration does not change how the radio sends or receives ASTRA frames. The radio code transmits bytes and checks framing. Calibration only matters to radio-side debugging when a developer is staring at raw bytes and wants to understand the real physical value. GSC uses the generated XML to decode the full frame and map it into YAMCS/Java-friendly data for storage and display.

## Source Artifacts

- ExternalResources/AV ASTRA google sheet/Parameters.html
- ExternalResources/AV ASTRA google sheet/Encoding Types.html
- telemetry-2026/telemetry/gen/telemetry_packets.h

## Related Pages

- [Telemetry atomics](../telemetry/atomics.md)
- [ASTRA frame format](../protocols/astra-frame-format.md)
- [Flight-stage atomic scheduling](../telemetry/flight-stage-atomic-scheduling.md)
- [Raw telemetry and metadata over MQTT](raw-telemetry-and-metadata-mqtt.md)

## Open Questions

- Where is the GSC XML generation script stored?
- What exact XML schema does GSC consume?
- Are calibration functions always written in reverse Polish notation?
- What is the authoritative rule for choosing an encoded integer type instead of float32?
- Which fields currently have finalized calibrations versus placeholder calibrations?
