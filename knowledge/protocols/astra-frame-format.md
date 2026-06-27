# ASTRA Frame Format

## Summary

An ASTRA telemetry frame is made of a fixed 8-byte header followed by a variable-length payload. The header identifies the packet, carries control flags, optionally acknowledges a command, and declares which atomic telemetry groups are present in the payload.

LoRa only transports the bytes. ASTRA defines how those bytes are structured and interpreted by the flight computer, ground station radios, and ground station control software.

## Frame Layout

| Section | Size | Purpose |
| --- | ---: | --- |
| Header | 8 bytes | Sequence number, flags, acknowledgement ID, and atomics bitmap. |
| Payload | Variable | Packed atomic telemetry groups selected by the bitmap. |

## Header Fields

| Offset | Field | Size | Type | Meaning |
| ---: | --- | ---: | --- | --- |
| 0 | Sequence number | 2 bytes | uint16 | Incremented when the flight computer generates/sends a packet. Also called packet ID. |
| 2 | Flags | 1 byte | uint8 | Bit flags for CTS, ACK, BAD, and NAK behavior. |
| 3 | ACK ID | 1 byte | uint8 | Command ID being acknowledged when ACK is set. Current implementation uses 0 when unused. |
| 4 | Atomics bitmap | 4 bytes | uint32 | Bit i means atomic i is present in the payload. |

## Flags Byte

Bit numbering starts at bit 0, the least significant bit.

| Bit | Mask | Name | Meaning |
| ---: | ---: | --- | --- |
| 0 | 0x01 | CTS | Clear To Send. The flight computer is telling the ground radios they may send the next command. After sending CTS, the FC enters receive mode. |
| 1 | 0x02 | ACK | This packet contains an acknowledgement for a command sent by the ground side. |
| 2 | 0x04 | BAD | The FC received a command during the last CTS opportunity, but did not understand it or could not perform it. |
| 3 | 0x08 | NAK | No-op acknowledgement. Used when the FC asked for a command, but the ground side only had a NOP to send. This verifies the FC received the NOP without treating it as a real command ACK. |
| 4-7 | Unknown | Reserved | Not currently documented. |

## Variable Payload

The payload contains zero or more atomic telemetry groups. The atomics bitmap declares which groups are present.

Example: if the bitmap is binary 101, atomic 0 and atomic 2 are present. Atomic payloads are ordered by increasing atomic index, so atomic 0 appears before atomic 2 in the payload.

The current implementation calculates each atomic payload offset by summing the sizes of all lower-numbered atomics that are present in the bitmap. This means every system that decodes ASTRA frames must share the same atomic definitions and sizes. Packet size is intentionally kept low where possible because larger packets have higher transmit latency and more opportunity for RF corruption or loss.

## Implementation Notes from telemetry-2026

The inspected telemetry implementation defines:

- Frame header struct: telemetry/fixed/frame_header.h.
- Header size assertion: 8 bytes.
- Maximum bitmap width: 32 atomic packets.
- Payload length and atomic offsets: telemetry/fixed/frame_helper.cpp.
- Frame building order: telemetry/fixed/frame_builder.cpp iterates atomics from low index to high index.

## Related Pages

- [ASTRA protocol](astra.md)
- [ASTRA command packets](astra-command-packets.md)
- [Telemetry atomics](../telemetry/atomics.md)
- [Flight-stage atomic scheduling](../telemetry/flight-stage-atomic-scheduling.md)
- [GSC telemetry calibration](../ground-station/gsc-telemetry-calibration.md)
- [System overview](../context/system-overview.md)

## Open Questions

- What is the intended sequence-number wrap behavior after 65535?
- Should ASTRA explicitly specify byte order, or does the project rely on matching little-endian targets?
- Are bits 4-7 of the flags byte reserved permanently, or are any planned for Launch Canada 2026?
- Does the ground side ever generate sequence numbers, or is the sequence number only an FC telemetry packet ID?
