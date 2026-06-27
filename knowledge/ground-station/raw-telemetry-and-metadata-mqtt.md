# Raw Telemetry and Metadata over MQTT

## Summary

Ground station radios publish raw ASTRA frame bytes over MQTT to keep the path fast and avoid duplicating telemetry parsing logic inside the radio firmware.

GSC receives the raw bytes, decodes the ASTRA frame using generated XML definitions, maps the values into YAMCS/Java-friendly data, persists the data in a local database, and displays selected values in the frontend. The database and frontend behavior are outside the radio knowledge scope.

## Raw ASTRA Payload

When a ground station radio receives a valid LoRa packet from the rocket, it validates the byte array as ASTRA and publishes the raw frame bytes to the rocket telemetry MQTT topic.

The radio does not expand the ASTRA atomics into GUI values. That decode belongs to GSC because GSC already has the XML and calibration metadata.

## Matching Radio Metadata

Each time the radio receives telemetry, it also publishes a matching radio metadata payload. This lets operators debug link quality alongside the telemetry data.

Current metadata fields:

| Field | Size | Meaning | Decode |
| --- | ---: | --- | --- |
| seq | 2 bytes | ASTRA sequence number for the corresponding telemetry frame. | uint16 |
| radio_rssi | 1 byte | Raw RSSI register-style value from the ground station radio. | Real RSSI is radio_rssi / -2.0. |
| radio_snr | 1 byte | Raw SNR register-style value from the ground station radio. | Real SNR is radio_snr / 4. |

The sequence number ties the link-quality metadata back to the telemetry frame it describes.

## Why Raw Bytes Are Used

- Faster radio firmware path.
- Avoids adding GUI parsing/calibration responsibility to the radio.
- Keeps ASTRA decode centralized in GSC XML/YAMCS tooling.
- Preserves exact received payload for debugging.

## Related Pages

- [Ground station radio network and MQTT](radio-network-and-mqtt.md)
- [GSC telemetry calibration](gsc-telemetry-calibration.md)
- [ASTRA frame format](../protocols/astra-frame-format.md)
- [Telemetry atomics](../telemetry/atomics.md)

## Open Questions

- What exact MQTT payload type does GSC expect for raw ASTRA bytes: binary payload, byte array wrapper, or another container?
- What exact schema does GSC use for radio metadata ingestion?
- How does GSC pair late or missing metadata with telemetry frames?
