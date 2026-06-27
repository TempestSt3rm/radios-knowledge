# Knowledge Map

This map defines the intended structure of the knowledge base. It is a guide, not a rigid rule. Add pages only when there is real information to store.

## Top-Level Structure

~~~text
radios-knowledge/
  AGENTS.md
  README.md
  docs/
    knowledge-map.md
    style-guide.md
    intake-template.md
  knowledge/
    context/
    telemetry/
    protocols/
    radios/
    ground-station/
    operations/
    testing/
    troubleshooting/
    decisions/
~~~

## Subject Areas

| Area | Use For | Avoid Putting Here |
| --- | --- | --- |
| knowledge/context/ | Shared definitions, system overview, ownership boundaries, and concepts that apply across telemetry, radios, and ground station. | Deep implementation details that belong in a specific subject page. |
| knowledge/telemetry/ | Data sent from the rocket, packet fields, status messages, logging, data flow, and telemetry-specific software behavior. | Radio hardware details unless they directly affect telemetry encoding or transport. |
| knowledge/protocols/ | ASTRA and other project-specific protocol layers above raw LoRa bytes. | RF theory or physical radio hardware details. |
| knowledge/radios/ | Radio modules, LoRa configuration, firmware behavior, antennas, frequencies, link assumptions, radio hats, and radio-side procedures. | Ground station UI or operator workflow unless it is specifically radio setup. |
| knowledge/ground-station/ | Receive station hardware, software, operator displays, storage, networking, and launch-day setup. | Rocket-side telemetry details unless needed to explain what the station receives. |
| knowledge/operations/ | Launch-day procedures, field setup, roles, checklists, and recovery workflows. | Long explanations of system internals. Link to subject pages instead. |
| knowledge/testing/ | Test campaigns, validation steps, range checks, bench tests, and results. | Final operating procedures unless the page is explicitly about testing. |
| knowledge/troubleshooting/ | Symptoms, likely causes, diagnostic steps, and fixes. | Background theory unless needed for diagnosis. |
| knowledge/decisions/ | Design decisions, alternatives considered, tradeoffs, and rationale. | Raw meeting notes without conclusions. |

## Current Pages

- [System overview](../knowledge/context/system-overview.md)
- [Launch Canada requirement notes](../knowledge/context/launch-canada-requirement-notes.md)
- [Telemetry overview](../knowledge/telemetry/overview.md)
- [ASTRA protocol](../knowledge/protocols/astra.md)
- [ASTRA frame format](../knowledge/protocols/astra-frame-format.md)
- [ASTRA command packets](../knowledge/protocols/astra-command-packets.md)
- [ASTRA runtime behavior](../knowledge/protocols/astra-runtime-behavior.md)
- [Telemetry atomics](../knowledge/telemetry/atomics.md)
- [Flight-stage atomic scheduling](../knowledge/telemetry/flight-stage-atomic-scheduling.md)
- [GSC telemetry calibration](../knowledge/ground-station/gsc-telemetry-calibration.md)
- [Ground station radio network and MQTT](../knowledge/ground-station/radio-network-and-mqtt.md)
- [Raw telemetry and metadata over MQTT](../knowledge/ground-station/raw-telemetry-and-metadata-mqtt.md)
- [LoRa radio link](../knowledge/radios/lora-link.md)
- [Radio configuration](../knowledge/radios/configuration.md)
- [Radio startup behavior](../knowledge/radios/radio-startup-behavior.md)
- [Radio command interface](../knowledge/radios/radio-command-interface.md)
- [Antennas](../knowledge/radios/antennas.md)
- [April 2026 range test](../knowledge/testing/range-test-april-2026.md)
- [Radio hat hardware](../knowledge/radios/radio-hat-hardware.md)

## Suggested Future Pages

Create these when enough real information exists:

- knowledge/telemetry/field-list.md
- knowledge/ground-station/overview.md
- knowledge/ground-station/operator-workflow.md
- knowledge/ground-station/mqtt-topic-reference.md
- knowledge/operations/launch-canada-requirements-official.md
- knowledge/operations/launch-canada-2026-setup.md
- knowledge/troubleshooting/no-telemetry-received.md
- knowledge/decisions/radio-system-design.md

## Linking Rules

- Link to the source page instead of copying the same explanation.
- If one fact matters in multiple areas, store it once in the most specific page and link to it.
- Use short descriptive link text, such as radio configuration, when linking to a real page.
- Update this map when adding a new major page or folder.
