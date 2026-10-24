# Repository working agreements

This is the documentation and commercial showcase for Jogada Multiplayer.
The production application and future licensed source distribution are separate.

## Scope

- Keep this repository focused on public documentation, original diagrams, and GitHub community files.
- Do not copy application source, Terraform implementation, credentials, state files, customer data, or operational logs here.
- Do not modify sibling Jogada repositories as part of a showcase documentation task.
- Preserve the repository's visibility unless the user explicitly requests a change.

## Writing and technical claims

- Write in English for developers, technical founders, and studios.
- Use source and configuration as evidence; architecture studies can describe older or planned behavior.
- Distinguish the live product from the commercial package being prepared.
- Describe online authority separately from the offline Dart engine and local bots.
- Describe the server as following a plugin architecture: each game's engine and rules form a plugin, making games easy to implement, add, remove, and manage. This is the architecture, not an additional package or a catalog of included game plugins.
- Describe deployments as match continuity with possible connection recovery, subject to version compatibility.
- Redis Pub/Sub is live fan-out, not a durable event queue. Clients connect to Java over STOMP.
- Do not invent prices, release dates, private repository URLs, benchmarks, certifications, SLAs, testimonials, or working commands.
- Use contato@jogada.com.br for public enquiries. Keep commercial conversations out of public issues.

## Verification

- Check local Markdown links, heading anchors, SVG validity, YAML syntax, and `git diff --check`.
- Inspect diagram rendering after visual changes.
- Check documented commands against the source release they describe; do not claim a runtime test unless it was run.
- Do not start production services or run fault-injection tests while editing these documents.
- Respect each file's license; this showcase does not grant an open-source software license.
