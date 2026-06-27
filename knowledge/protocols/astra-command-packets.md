# ASTRA Command Packets and Acknowledgements

## Summary

Commands are messages sent from the ground station to the rocket. ASTRA uses command IDs and acknowledgement fields so the ground side can verify which command was received, especially when the RF link is lossy.

## Command Packet Concept

A command packet contains:

| Field | Size | Meaning |
| --- | ---: | --- |
| command_id | 1 byte | Ground-generated ID used to match a command to a later acknowledgement. |
| command string | Needs confirmation | Short command code, such as the propulsion command example pi. |
| optional data | Variable | Extra command arguments when needed. |

The command ID is copied into the ASTRA header ACK ID when the FC acknowledges that command. This lets the ground station control computer link a specific transmitted command to a specific acknowledgement.

## Example

If operators send the same propulsion command multiple times over a lossy link, the ground station control computer can assign different command IDs:

| Sent Command | Meaning |
| --- | --- |
| 1, pi | First attempt or instance of the pi command. |
| 2, pi | Second attempt or instance of the pi command. |

When the FC later sends an ACK with ACK ID 2, the ground side knows exactly which command made it through.

## CTS and Command Opportunity

The radios are half-duplex, so the rocket and ground station should not transmit at the same time. The FC is the leader and the ground station radio is the follower. The CTS flag creates a command opportunity:

1. The FC sends an ASTRA frame with CTS set.
2. After sending CTS, the FC enters receive mode.
3. If a command is queued and TX-from-CTS is enabled on that radio, the ground radio sends it.
4. If no command is queued, the ground radio sends a NOP.
5. After transmitting, the ground radio immediately returns to receive mode.
6. The FC later reports the result with ACK, BAD, or NAK behavior.

## Acknowledgement Flags

| Flag | Meaning |
| --- | --- |
| ACK | A real command was received and acknowledged. The ACK ID identifies which command. |
| BAD | A command was received during the last CTS window, but the FC did not understand it or could not perform it. |
| NAK | A NOP was received when no operator command was queued. This confirms the command window worked without treating the NOP as a real command ACK. |

## Implementation Notes from telemetry-2026

The inspected command packet implementation defines:

- Base command packet: uint8 command_id plus a fixed command string buffer.
- Extended command packet: base command packet plus argc and up to three float arguments.
- Frame validation distinguishes ASTRA telemetry frames from command packet lengths.

There is one detail to confirm: the verbal description says the command string is 5 bytes, while the inspected C++ code defines char command_string[6] and comments that the string must be null terminated.

## Launch Canada Requirement Note

Command acknowledgement appears to be important for Launch Canada requirements, especially as final proof that a command was received and acknowledged over a lossy RF link. This requirement should be confirmed against the official LC documentation or team requirement tracker before being treated as a formal rule.

## Runtime Behavior

In normal operation the FC sends telemetry continuously and only periodically opens a command opportunity with CTS. See [ASTRA runtime behavior](astra-runtime-behavior.md).

## Related Pages

- [ASTRA protocol](astra.md)
- [ASTRA frame format](astra-frame-format.md)
- [System overview](../context/system-overview.md)

## Open Questions

- What is the exact command string length and null-termination rule?
- What is the full command list for Launch Canada 2026?
- What does the pi command stand for in final operator-facing terminology?
- Does BAD include the command ID that failed, or only indicate that the last command window failed?
- Is NOP assigned a command ID, or is NAK independent of ACK ID?
