# Maintainer launch checklist

This is a preparation checklist, not a record of completed commercial release
validation. Keep the repository private until the owner requests publication.

## Showcase foundation

- [x] Write the English README, architecture, local-development preview, evaluation guide, licensing page, and FAQ.
- [x] Add an original architecture diagram and a proprietary rights notice.
- [x] Use contato@jogada.com.br, already listed on the public Jogada website, for commercial enquiries.
- [x] Separate showcase contents from application source and infrastructure implementation.
- [x] Use the real production game URL and actual source-workspace commands rather than invented clone links or launchers.
- [x] Add documentation feedback and private-reporting instructions.

## Before making the showcase public

- [ ] Confirm the copyright owner shown in LICENSE.md; it currently names Manoel Castillo.
- [ ] Confirm the contact inbox receives licensing enquiries and has an appropriate owner.
- [ ] Review every capability against the source release intended for distribution.
- [ ] Decide whether to publish the current early-access positioning or wait for a purchasable release.
- [ ] Add an approved recording demonstrating worker replacement and client recovery, or retain the explicit statement that no recording is published yet.
- [ ] Review the rendered README, diagram, all links, and the GitHub issue form.
- [ ] Review the staged files and history for material that does not belong in a public showcase.
- [ ] Set the GitHub About description, website, and topics.
- [ ] Change visibility only when explicitly requested by the repository owner.

## Before accepting a commercial purchase

- [ ] Prepare a clean private source distribution without production credentials, customer records, or operational history.
- [ ] Verify installation from a fresh workspace using only the release's documented prerequisites.
- [ ] Exercise cross-worker play, reconnection, worker replacement, and a compatible rollout for the offered versions.
- [ ] Confirm native platform, offline, and billing claims against the release being sold.
- [ ] Verify the game engine plugin architecture, add/remove/manage behavior, and integration documentation before delivering any licensed server.
- [ ] Finalize license scope, pricing, update/access duration, renewal conditions, and support boundaries.
- [ ] Confirm rights to distribute all included source, assets, dependencies, and infrastructure tooling.
- [ ] Prepare payment and source-access fulfillment; test an invitation with the intended buyer permissions.
- [ ] Prefer an organization-owned private source repository when buyers should receive read-only access.
- [ ] Publish exact release contents, installation instructions, and exclusions before purchase.

## Suggested GitHub About settings

Description:

> Production-proven multiplayer infrastructure for card and turn-based games: Flutter, authoritative Java servers, Redis, and AWS Fargate/Terraform. Architecture, demos, and commercial licensing.

Website: `https://jogada.com.br/`

Topics: `flutter`, `dart`, `java`, `spring-boot`, `redis`, `websocket`, `stomp`,
`multiplayer`, `card-game`, `aws-fargate`, `terraform`, `self-hosted`.

Future distribution shape is a packaging decision. Do not publish fictional
Core/Plugins directories or a private repository URL before they exist.
