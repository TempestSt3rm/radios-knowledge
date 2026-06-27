# Radio Hat Hardware

## Summary

The radio hat PCB is the hardware interface between the host computer and the LoRa radio module. The same radio hat concept is used on both the rocket-side flight computers and the ground station radios.

## Hardware Stack

| Component | Role |
| --- | --- |
| Semtech SX1262/SX1268 | LoRa radio IC that handles RF modulation, demodulation, error correction, and packet integrity checks. |
| Dorji SX126x shield | Purchased radio shield/module mounted to the radio hat PCB. Boosts transmit power to 30 dBm. |
| Radio hat PCB | Custom PCB used to integrate the radio shield with FC or ground station host hardware. |
| Flight computer host | STM32-based rocket-side system. |
| Ground station host | Teensy 4.1-based ground-side system. |

## Flight Computer Side

The flight computer is STM32-based. It generates telemetry and uses radio-side software to format telemetry bytes into ASTRA frames before transmission.

## Ground Station Side

The ground station radio uses a Teensy 4.1. Teensy 4.1 was chosen on the ground station side because it can communicate over Ethernet, which lets the radio forward received telemetry over LAN to the ground station computer.

## Driver Differences

Both the flight computer and ground station sides operate using ASTRA-style frames, but their radio drivers differ because the host CPUs are different. Low-level wiring details are not a priority for this knowledge base unless they are needed for debugging or integration.

## Power Notes

The radio hat draws about 2.5 W at maximum. Range tests were conducted with micro USB power from a computer. The planned PoE path has comfortable margin: PoE can provide about 10 W at 5 V / 2 A through the splitter, which is well above the radio hat requirement.

## Related Pages

- [LoRa radio link](lora-link.md)
- [ASTRA protocol](../protocols/astra.md)
- [System overview](../context/system-overview.md)

## Open Questions

- What is the exact radio hat PCB revision used for Launch Canada 2026?
- What are the main driver differences between STM32 and Teensy 4.1?
- What Ethernet protocol or packet format does the ground station Teensy use to send telemetry over LAN?
