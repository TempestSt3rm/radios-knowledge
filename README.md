# Radios Knowledge

This repository is the knowledge base for the McGill Rocket Team telemetry, radios, and ground station setup designed for Launch Canada 2026.

The goal is to turn spoken or written explanations into clear Markdown pages that a non-expert can consult like a small technical wiki. The repository should capture what was built, why decisions were made, how the system works, and what future team members need to know to operate, debug, or extend it.

## Core Areas

- Telemetry: data sent from the rocket, packet formats, logging, health/status signals, and integration points.
- Radios: radio hardware, firmware behavior, frequencies, link budget assumptions, antennas, configuration, and operating procedures.
- Ground station: receive-side hardware, software flow, displays, storage, operator workflow, and launch-day setup.
- Launch Canada 2026 context: requirements, constraints, field setup, tests, decisions, and lessons learned.

## How This Repo Should Be Used

An AI assistant should help maintain this repository by recording new information in structured Markdown. It should first read the existing Markdown files, find the right place for the new information, avoid repetition, and ask follow-up questions when details are missing or uncertain.

This repo should prefer concise, factual notes over long narrative dumps. When something is unknown, mark it as unknown instead of guessing.

## Start Here

- [AI instructions](AGENTS.md)
- [Knowledge map](docs/knowledge-map.md)
- [Writing style guide](docs/style-guide.md)
- [Intake template](docs/intake-template.md)
