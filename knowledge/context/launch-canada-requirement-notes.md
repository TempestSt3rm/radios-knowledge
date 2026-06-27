# Launch Canada Requirement Notes

## Summary

This page records Launch Canada (LC) requirement-related notes that matter to radios and telemetry. It is not a full copy of the LC rulebook.

## Frequency Agility

The radio system should be able to change RF center frequencies. Frequency approval at LC is expected to be ad-hoc during the competition, so the important system capability is runtime frequency agility.

The selected frequency must remain within the allowed amateur radio band for the hardware and operations plan. The radio software supports changing the tuned frequency within the band selected by the radio hat.

## Command Acknowledgement

Command acknowledgement is important because it lets the ground station prove that a command was received by the flight computer over a lossy RF link. This is believed to be connected to LC expectations for safety-critical command handling, but the exact official requirement should be confirmed from LC documentation.

## Scope

Keep this page limited to LC requirements that affect radios, telemetry, ASTRA, RF behavior, or ground-station radio operation. Do not expand it into a general LC competition rule summary.

## Related Pages

- [Radio configuration](../radios/configuration.md)
- [ASTRA command packets](../protocols/astra-command-packets.md)
- [ASTRA runtime behavior](../protocols/astra-runtime-behavior.md)

## Open Questions

- What is the exact LC wording for command acknowledgement, if any?
- Are there LC-specific restrictions on transmit power, operators, call signs, or band use that should be recorded here?
