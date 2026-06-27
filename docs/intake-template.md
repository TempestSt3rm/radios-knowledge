# Knowledge Intake Template

Use this template when turning a conversation, meeting note, or explanation into repository knowledge.

## Intake Checklist

Before writing:

- Read the relevant existing Markdown pages.
- Check whether the information already exists.
- Identify the best target page or folder.
- Ask the user for missing details if the gap affects correctness.

## Source Note

~~~markdown
Source:
- User:
- Date:
- Context:
~~~

## Capture Draft

~~~markdown
# Topic

## Summary

What this is and why it matters.

## Confirmed Information

- Confirmed fact.
- Confirmed fact.

## Details

| Item | Value | Notes |
| --- | --- | --- |
| Unknown | Unknown | Needs confirmation. |

## Related Pages

- Related page path: ../path/example.md

## Open Questions

- What needs to be clarified?
~~~

## Follow-Up Question Patterns

Use direct questions like:

- "What exact radio module did you use for this part of the system?"
- "Is this value a Launch Canada 2026 value, or is it from an earlier test?"
- "Should this be documented as the final design or as an abandoned option?"
- "Where does this data get logged on the ground station?"
- "What symptom would an operator see if this failed?"

## Final Pass

After writing:

- Remove duplicated wording.
- Link related pages.
- Keep only useful open questions.
- Make sure no placeholder sections remain unless they intentionally mark unknowns.
