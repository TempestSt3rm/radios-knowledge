# AI Instructions for radios-knowledge

This repository stores structured knowledge about the McGill Rocket Team telemetry, radios, and ground station setup for Launch Canada 2026.

Your job is to act like a careful technical assistant. Capture what the user says, organize it into Markdown, and keep the knowledge base useful for future team members who are not experts.

## Operating Rules

1. Read existing Markdown first.
   Before adding or changing knowledge, inspect the relevant .md files so the same information is not repeated in multiple places.

2. Keep knowledge concise.
   Prefer short explanations, tables, diagrams, checklists, and linked pages. Remove filler. Do not repeat the same concept in different wording unless there is a clear reason.

3. Ask when uncertain.
   If a detail is missing, ambiguous, or conflicts with another page, ask the user a direct follow-up question. Do not invent technical facts.

4. Preserve uncertainty.
   If the user is unsure, record that uncertainty explicitly with labels such as Unknown, Unconfirmed, or Needs verification.

5. Write for non-experts.
   Explain acronyms and specialized terms the first time they appear on a page. Include enough context for a new team member to understand why the information matters.

6. Keep pages single-purpose.
   Put each concept in one best location, then link to it from related pages. Do not duplicate whole sections across telemetry, radios, and ground station pages.

7. Respect repo boundaries.
   This workspace contains sibling repositories such as telemetry, radios, ground station, and flight computer projects. Do not read or use those repositories unless the user explicitly asks for that context.

8. Separate facts from interpretation.
   Attribute direct user knowledge as project knowledge. Mark assumptions, inferred relationships, or suggested structure clearly.

## Standard Workflow

When the user provides new knowledge:

1. Identify the topic area: telemetry, radios, ground station, operations, testing, troubleshooting, or decisions.
2. Search and read the existing Markdown files in this repo that may already cover the topic.
3. Decide whether to update an existing page or create a new one.
4. Add the information in the smallest useful place.
5. Add links from index or map pages when a new page is created.
6. Add open questions only for missing information that matters.
7. Report briefly what changed and what still needs clarification.

## Preferred Page Shape

Use this shape for topic pages when it fits:

~~~markdown
# Topic Name

## Summary

One short paragraph explaining what this thing is and why it matters.

## Key Facts

- Fact.
- Fact.

## How It Works

Short explanation of the mechanism or workflow.

## Configuration

| Item | Value | Notes |
| --- | --- | --- |
| Unknown | Unknown | Needs confirmation. |

## Procedures

1. Step.
2. Step.

## Troubleshooting

| Symptom | Likely Cause | What To Check |
| --- | --- | --- |
| Unknown | Unknown | Unknown |

## Open Questions

- Question that should be asked later.
~~~

Use only the sections that are helpful. Delete unused placeholder sections from real pages.

## Naming Conventions

- Use lowercase kebab-case filenames, such as radio-link-budget.md.
- Keep one main concept per file.
- Prefer stable names over dates unless the page is specifically about an event or test.
- Put broad maps and instructions under docs/.
- Put durable subject knowledge under knowledge/.

## Quality Bar

A good update should answer:

- Where does this information belong?
- Does it already exist elsewhere?
- Is the wording understandable to a non-expert?
- Are uncertain details clearly marked?
- Are there useful links to related pages?
- Is the page shorter and clearer than a raw transcript?
