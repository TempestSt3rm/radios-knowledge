# ASTRA Protocol

## Summary

ASTRA stands for Avionics Standard for Radios Telemetry Adoption. It is the project telemetry and command protocol layer that sits above LoRa.

LoRa handles the lower-level RF work, including modulation, demodulation, RF encoding, error correction, and packet corruption checks. ASTRA handles the project-specific structure and behavior for telemetry and commands.

## Why ASTRA Exists

The radios are half-duplex: the rocket and ground station can both use the RF link, but they should not transmit at the same time. RF packets can also be dropped, disappear, or become corrupted in transmission.

ASTRA exists so the system can define:

- How telemetry bytes are formatted into frames.
- How commands are formatted into packets.
- When telemetry should be sent.
- When commands should be sent.
- Which side leads the half-duplex exchange.
- What counts as acknowledged.
- What counts as dropped.
- How the system behaves when packets are lost.

## Core Concepts

| Concept | Meaning |
| --- | --- |
| ASTRA frame | Telemetry frame with an 8-byte header and variable-length atomic payload. |
| Header | Fixed metadata containing sequence number, flags, ACK ID, and atomics bitmap. |
| Atomic | Fixed group of telemetry fields that can appear in the variable payload. |
| Calibration | GSC-side conversion from compact raw telemetry encoding into operator-facing values and units. |
| Command packet | Ground-to-rocket packet containing a command ID, command string, and optional data. |
| ACK ID | Field used to match a command acknowledgement to a specific command ID. |
| CTS | Clear To Send flag. Creates a command opportunity for the ground side. |
| RXTX ratio | Number of FC telemetry transmissions between command opportunities. Current intended value is 4. |
| TX-from-CTS | Ground-radio setting that controls whether the radio may transmit a queued command when CTS arrives. |

## Layering

| Layer | Responsibility |
| --- | --- |
| Application data | Rocket telemetry values and ground-to-rocket commands. |
| ASTRA | Project framing, command/telemetry structure, acknowledgements, and drop behavior. |
| Radio driver | CPU-specific interface between software and the radio hat. |
| LoRa/SX126x | RF modulation, demodulation, error correction, and packet integrity checks. |
| RF link | Physical wireless transmission between rocket and ground station. |

## Telemetry and Command Direction

| Direction | Message Type | Example |
| --- | --- | --- |
| Rocket to ground | Telemetry frame | Altitude, vertical speed, payload data, propulsion valve states. |
| Ground to rocket | Command packet | LAUNCH command or propulsion command. |

## Important Distinction

LoRa lets the system transmit and receive arrays of bytes over RF. ASTRA defines what those bytes mean to the McGill Rocket Team system.

## Related Pages

- [ASTRA frame format](astra-frame-format.md)
- [ASTRA command packets](astra-command-packets.md)
- [ASTRA runtime behavior](astra-runtime-behavior.md)
- [Telemetry atomics](../telemetry/atomics.md)
- [Flight-stage atomic scheduling](../telemetry/flight-stage-atomic-scheduling.md)
- [GSC telemetry calibration](../ground-station/gsc-telemetry-calibration.md)
- [System overview](../context/system-overview.md)
- [Telemetry overview](../telemetry/overview.md)
- [LoRa radio link](../radios/lora-link.md)

## Open Questions

- What is the full command list for Launch Canada 2026?
- Is RXTX ratio 4 final for Launch Canada 2026?
- Does ASTRA use retransmission, priority, time slots, or another scheduling method beyond CTS command opportunities?
