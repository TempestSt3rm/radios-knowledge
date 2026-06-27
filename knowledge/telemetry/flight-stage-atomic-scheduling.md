# Flight-Stage Atomic Scheduling

## Summary

The set of atomics sent by the flight computer changes based on flight stage. This lets ASTRA reduce packet size and telemetry load as the rocket gets farther from the ground station.

The packet rates in the H26 AV packet coordinator are estimated packet reception frequencies. The estimates are based on data from the previous URRG launch with a Commercial Off The Shelf (COTS) engine in upstate New York.

## Design Rationale

Larger packets take longer to transmit and are more exposed to RF corruption or loss. During the URRG flight, the system sent a near-full 85-byte payload continuously. Reception frequency decreased as the rocket moved farther away, but telemetry was still received after landing more than 1 km away.

For Launch Canada 2026, the plan is to reduce telemetry load by sending only the atomics needed for each flight stage.

## Current Sheet Schedule

| Flight Stage | Description | Atomics | Packet Rate |
| --- | --- | --- | ---: |
| 0=Pad | Pre-launch, while the rocket is on the ground. | prop_atomic, recov_atomic, prop_states_atomic, fc_internal_atomic, gyro_atomic, acceleration_atomic, altitude_atomic, gps_atomic, radio_atomic, sd_atomic, flight_stage_atomic | 4 p/s |
| 1=Ascent | Rocket accelerating upwards, engine on, accel > 1 g. | gps_atomic, recov_atomic, prop_states_atomic, prop_atomic, flight_stage_atomic, acceleration_atomic, altitude_atomic | 3 p/s |
| 2=Pre-Apogee | Coasting phase, rocket ascending, engine off. | gps_atomic, recov_atomic, prop_states_atomic, prop_atomic, flight_stage_atomic, acceleration_atomic, altitude_atomic | 3 p/s |
| 3=Drogue descent | Rocket has deployed drogue at apogee and is descending. | gps_atomic, recov_atomic, flight_stage_atomic, acceleration_atomic, altitude_atomic, altitude_events_atomic, fc_internal_atomic | 3 p/s |
| 4=Main descent | Rocket has deployed main at target altitude and is descending slower. | gps_atomic, recov_atomic, flight_stage_atomic, acceleration_atomic, altitude_atomic, altitude_events_atomic, fc_internal_atomic | 2 p/s |
| 5=Landed | Rocket is on the ground. | gps_atomic, flight_stage_atomic, altitude_events_atomic, fc_internal_atomic | Unknown |

## Source Artifacts

- ExternalResources/AV ASTRA google sheet/Atomic Frequency.html
- ExternalResources/AV ASTRA google sheet/Full Packet.html

## Related Pages

- [Telemetry atomics](atomics.md)
- [GSC telemetry calibration](../ground-station/gsc-telemetry-calibration.md)
- [ASTRA frame format](../protocols/astra-frame-format.md)
- [April 2026 range test](../testing/range-test-april-2026.md)

## Open Questions

- Are the listed packet rates transmit rates, expected receive rates, or both in final LC terminology?
- How does the FC decide the current flight stage for telemetry scheduling?
- Are any atomics sent at lower frequency within a stage, or is each listed atomic included in every scheduled packet?
- What is the target payload size for each stage after optimization?
- What should the landed packet rate be?
