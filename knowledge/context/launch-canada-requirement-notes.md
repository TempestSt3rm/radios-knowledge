# Launch Canada Requirement Notes

## Summary

This page records Launch Canada (LC) requirement-related notes that matter to radios and telemetry. It is not a full copy of the LC rulebook.

## Frequency Agility

The radio system should be able to change RF center frequencies. The current understanding is that LC expects teams to be able to adjust RF frequencies, as long as the selected frequencies remain within the allowed amateur radio bands for the hardware and operations plan.

This matters because the system should not hard-code a single immutable center frequency. The radio software supports changing the tuned frequency within the band selected by the radio hat.

## Command Acknowledgement

Command acknowledgement is important because it lets the ground station prove that a command was received by the flight computer over a lossy RF link. This is believed to be connected to LC expectations for safety-critical command handling, but the exact official requirement should be confirmed from LC documentation.

## Scope

Keep this page limited to LC requirements that affect radios, telemetry, ASTRA, RF behavior, or ground-station radio operation. Do not expand it into a general LC competition rule summary.

## Related Pages

- [Radio configuration](../radios/configuration.md)
- [ASTRA command packets](../protocols/astra-command-packets.md)
- [ASTRA runtime behavior](../protocols/astra-runtime-behavior.md)

## Open Questions

- What is the exact LC wording for frequency agility or RF frequency changes?
- What is the exact LC wording for command acknowledgement, if any?
- Are there LC-specific restrictions on transmit power, operators, call signs, or band use that should be recorded here?
