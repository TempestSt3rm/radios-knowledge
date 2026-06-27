# ASTRA Runtime Behavior

## Summary

ASTRA uses an FC-led, ground-station-following procedure. The flight computer decides when telemetry is sent and when the ground station is allowed to transmit a command.

This leader/follower behavior exists because the radios are half-duplex. If both sides transmit at the same time, the RF link can collide or lose data.

## Roles

| System | Role | Behavior |
| --- | --- | --- |
| Flight computer (FC) | Leader | Sends telemetry, periodically opens command opportunities with CTS, and returns to telemetry if no command arrives. |
| Ground station radio | Follower | Starts in receive mode, forwards telemetry to GSC, and only transmits a rocket command after receiving CTS and if TX-from-CTS is enabled. |
| Ground Station Control (GSC) | Command source and telemetry consumer | Publishes commands to MQTT and parses raw telemetry using ASTRA/XML definitions. It does not directly manage CTS timing. |

## Normal Telemetry Cycle

The FC normally sends telemetry continuously. It follows an RXTX ratio.

Current intended RXTX ratio: 4.

That means the FC sends telemetry packets, and every fourth telemetry packet includes CTS. CTS stands for Clear To Send and gives the ground station radio a chance to transmit one queued command.

## CTS Command Window

1. The FC sends telemetry packets according to the RXTX ratio.
2. On the CTS packet, the FC sets the CTS flag.
3. After sending CTS, the FC switches into receive mode.
4. The FC waits up to 1 second for a ground-station response.
5. If it receives a command packet, it later acknowledges the command.
6. If it receives a NOP, it later sends NAK to show the NOP path worked without treating it as a real command ACK.
7. If it receives nothing before timeout, it returns to sending telemetry.

## Ground Station Radio Behavior

Ground station radios start in receive mode. In normal operation they:

1. Receive LoRa packets from the rocket.
2. Use ASTRA parsing to check whether the received byte array is ASTRA-compatible.
3. Forward the raw ASTRA frame bytes to GSC over MQTT.
4. Watch for the CTS flag.
5. If CTS is present and TX-from-CTS is enabled, transmit the next queued rocket command.
6. Immediately return to receive mode after transmitting.

If a ground station radio never receives the CTS packet, it never transmits the queued command during that command window.

## Command Queueing

GSC does not need to understand CTS timing. When an operator clicks a button or activates a command in the GUI, GSC publishes the command over MQTT. The ground station radio stores valid rocket commands in an internal queue.

When CTS arrives, the radio does not ask GSC for a command in real time. It transmits the next command already queued locally. This keeps the RF command window fast.

At RXTX ratio 4, the command path is expected to feel responsive. A command can usually be queued, transmitted after CTS, and return around the loop quickly, roughly within the 1-second CTS behavior window when the link is healthy.

## Drop and Timeout Defaults

The fallback behavior is simple:

| Case | FC Behavior | Ground Station Radio Behavior |
| --- | --- | --- |
| CTS packet is lost before reaching ground | Continues after timeout. | Never transmits because it never saw CTS. |
| Ground command packet is lost before reaching FC | Times out or does not ACK that command. | Returns to receive mode immediately after transmit. |
| FC receives nothing during CTS window | Returns to sending telemetry. | Stays in receive mode. |
| Radio has no queued command when CTS arrives | Sends NOP. | Returns to receive mode. |

In general, the FC defaults to sending telemetry and the ground station radio defaults to receiving.

## ASTRA Validation and GUI Forwarding

The ground station radio uses ASTRA validation before interpreting a received byte array. The current radios-2026 code uses FrameView::validate for this.

The ground station radio forwards the raw ASTRA frame bytes to GSC. GSC uses its ASTRA/XML definitions to parse the raw bytes into telemetry fields for the GUI.

Current radios-2026 behavior also force-sends non-command frames to the GUI even if ASTRA validation fails, while logging the bad frame. Command packets intercepted by the ground station are not forwarded as telemetry.

## Code Notes from radios-2026

Observed implementation details:

- GroundStationVariant starts the ground station radio and calls receive mode during initialization.
- GroundStation::handleReceivedPacket validates received packets with FrameView.
- GroundStation::sendTelemetryToGui publishes raw ASTRA bytes to MQTT.
- GroundStation::handleRocketCommand defaults to a NOP command if no queued rocket command exists.
- RadioModule::transmitBlocking switches back into receive mode after transmit.
- GroundStationStore::canTxFromCTS gates whether a radio is allowed to transmit when CTS arrives.

## Related Pages

- [ASTRA protocol](astra.md)
- [ASTRA frame format](astra-frame-format.md)
- [ASTRA command packets](astra-command-packets.md)
- [Ground station radio network and MQTT](../ground-station/radio-network-and-mqtt.md)
- [Raw telemetry and metadata over MQTT](../ground-station/raw-telemetry-and-metadata-mqtt.md)

## Open Questions

- Is RXTX ratio 4 final for Launch Canada 2026, or just the current default?
- The detailed FC-side finite state machine lives in the flight-computer RTOS radio task and is out of scope unless radio-side behavior needs debugging.
- Does the final FC retry CTS on timeout immediately, or return to non-CTS telemetry first?
- What exact timeout does GSC use before showing a command as not acknowledged?
