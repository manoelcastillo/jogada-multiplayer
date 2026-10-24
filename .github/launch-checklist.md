# Maintainer launch checklist

Updated 25 October 2026. The owner has published the showcase. Commercial source
delivery remains a separate release with its own acceptance gates.

## Showcase foundation

- [x] Write the English README, architecture, local-development preview, evaluation guide, licensing page, and FAQ.
- [x] Add an original architecture diagram and a proprietary rights notice.
- [x] Use contato@jogada.com.br, already listed on the public Jogada website, for commercial enquiries.
- [x] Separate showcase contents from application source and infrastructure implementation.
- [x] Use the real production game URL and actual source-workspace commands rather than invented clone links or launchers.
- [x] Add documentation feedback and private-reporting instructions.

## Public showcase and launch assets

- [ ] Confirm the copyright owner shown in LICENSE.md; it currently names Manoel Castillo.
- [ ] Confirm the contact inbox receives licensing enquiries and has an appropriate owner.
- [ ] Review every capability against the source release intended for distribution.
- [x] Publish the showcase with accurate early-access positioning; the owner made it public.
- [x] Record two real Flutter clients, worker loss, and client WebSocket recovery in an isolated local environment.
- [x] Publish the recording with source revisions, assertions, measured observations, and explicit limits.
- [x] Publish a static GitHub Pages video player with captions; verify native playback and mobile layout.
- [x] Add an engineering article explaining match continuity and deployment compatibility.
- [x] Set the GitHub About description, hosted demonstration website, and developer topics.
- [x] Prepare the original 1280×640 social preview image and editable SVG.
- [ ] Upload `assets/social-preview.png` in GitHub Settings → Social preview; the available browser session was signed out.
- [x] Prepare owner-only Product Hunt copy, announcements, qualification replies, a lead tracker, and a founding-offer proposal in the ignored local `.launch/` directory.
- [ ] Review and send the first technical announcements from the owner's selected profiles; none were sent by the agent.
- [ ] Conduct 3–5 qualified project conversations and record adoption requirements privately.

## Before accepting a commercial purchase

- [x] Prepare private committed-source archives outside this public repository, without Git history or ignored files, and record checksums. These are internal candidates, not delivery-approved releases.
- [x] Build and run matching server/client source from fresh extractions with isolated local data services.
- [ ] Sanitize and approve the final private distribution: production-specific configuration, integrations, assets, dependencies, and documentation.
- [ ] Verify installation on an independent clean machine using only the release's documented prerequisites.
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

Website: `https://manoelcastillo.github.io/jogada-multiplayer/`

Topics: `flutter`, `dart`, `java`, `spring-boot`, `redis`, `websocket`, `stomp`,
`multiplayer`, `card-game`, `aws-fargate`, `terraform`, `self-hosted`.

Future distribution shape is a packaging decision. Do not publish fictional
Core/Plugins directories or a private repository URL before they exist.

The owner-only draft offer is not a public price or binding license. Keep the
public enquiry path quote-based until the owner adopts final scope and terms.
