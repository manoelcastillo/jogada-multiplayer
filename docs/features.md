# Feature catalog and package scope

[Back to the overview](../README.md) · [Technology stack and operations](technology-stack.md) · [Licensing and access](licensing-and-access.md)

The commercial developer distribution is being prepared. This catalog records the scope of the intended first release. The reference product is the starting point; neutral extraction, plugin-contract acceptance, provider configuration, platform checks, and release validation remain necessary. The new tutorial is still to be implemented.

Full Platform inherits Core. Buyers implement their game's Java rules plugin, Flutter game experience, and separate Dart rules/bots if offline play is wanted. Generic module integration does not supply a finished version of every game.

## Runtime, development, and client foundation

| Capability | Core | Full Platform | Delivery and integration boundary |
| --- | :---: | :---: | --- |
| Java authoritative runtime and game-plugin architecture | Yes | Yes | Buyer supplies rules, validation, scoring, and transitions through the plugin boundary |
| Redis live state and Pub/Sub backplane | Yes | Yes | Redis is internal; clients connect to Java over STOMP |
| PostgreSQL application persistence | Yes | Yes | Distinct from live match state; game-specific records need integration |
| Lobbies, matchmaking, and individual private matches | Yes | Yes | Match rules and permitted configurations belong to the game's implementation |
| Connection recovery and match resumption | Yes | Yes | Recovery requires valid match/seat/session and compatible state/protocols |
| Flutter/Riverpod foundation for Android, iOS, Web/PWA | Yes | Yes | Buyer builds game-specific screens and configures each platform |
| Reusable components, animation/theme primitives, starter assets and editable files | Yes | Yes | Approved neutral assets and workflow documentation; no Jogada branding or custom design service |
| Local Docker tooling | Yes | Yes | Initial dependencies/images must be downloaded; optional external integrations need their own configuration |
| Terraform for AWS | Yes | Yes | Buyer-owned accounts, resources, configuration, and operating costs |
| Original card-game integration tutorial | Yes | Yes | Planned example of private hands, deck state, turn validation, online Java, Flutter, and separate Dart offline rules/simple bot |

Compatible rolling deployment is a system design property subject to version compatibility. The published local recovery recording does not replace an AWS rollout acceptance test or workload-specific capacity testing.

## Engineering and operator characteristics

The reference system also contains Flyway-managed SQL migrations with startup schema validation, Java 21 virtual-thread configuration, dependency-aware health probes, structured logs, CloudWatch alarms, protected Prometheus metrics, local Grafana dashboards, and backend CI coverage/vulnerability gates. Its AWS definitions include ECR image management, DNS/TLS, data-store options, secret injection, and storage/CDN modules.

These are engineering characteristics of the inspected reference, with configuration and release-validation requirements. They do not create additional product-module entitlements. See the [stack and operations inventory](technology-stack.md) for versions, source boundaries, infrastructure options, and client/tooling details.

## Player accounts and communication

| Capability | Core | Full Platform | Delivery and integration boundary |
| --- | :---: | :---: | --- |
| Authentication and guest accounts | Yes | Yes | Reference integrations include email/phone OTP and Google/Apple sign-in; release/provider configuration must be verified |
| Guest-to-registered account conversion | Yes | Yes | Verify supported conversion and retained progress in the extracted release |
| Basic profiles and avatars | Yes | Yes | Approved assets, uploads, and storage configuration; progression/history modules have their own scope |
| Account deletion and data export | Yes | Yes | Buyer adapts game-specific data and supplies its policies; no blanket compliance certification |
| Push notifications and in-app announcements | Yes | Yes | Buyer supplies provider accounts/configuration and game-specific triggers |
| In-app support requests and operator replies | Yes | Yes | The buyer's team answers its players through the panel; seller installation assistance is separate |
| Contacts, presence, social/chat workflows | — | Yes | Configured channels and reusable controls; game-specific references must be adapted |
| Persistent private rooms and membership/access controls | — | Yes | Communities that persist across matches; individual private matches are Core |
| Social/chat reporting, muting, blocking, and moderation | — | Yes | Associated operator controls belong with the social/chat module |

## Engagement, billing, and product reporting

| Capability | Core | Full Platform | Delivery and integration boundary |
| --- | :---: | :---: | --- |
| Achievements and rankings | — | Yes | Buyer defines game-specific scoring, triggers, and achievement objectives |
| Ranking events and daily-challenge support | — | Yes | Buyer supplies scoring/objectives/rules; no Buraco challenge content or general tournament bracket engine |
| Player-facing match history | — | Yes | Record schema, retention, and visibility require release configuration |
| Completed-match replay support | — | Yes | Buyer adapts replay payloads/rendering; Jogada's Buraco replay screens are excluded |
| Stripe web/PWA billing workflows | — | Yes | Provider accounts, products, webhook configuration, and sandbox acceptance required |
| Native IAP integration | — | Yes | Google Play Billing / Apple StoreKit workflows with buyer-owned store configuration and release verification |
| Billing entitlements and related operator workflows | — | Yes | Adapt product/trial/paywall rules to the buyer's business model |
| Product analytics: activity, retention/cohorts, acquisition reports | — | Yes | Configure game events and external acquisition/spend inputs; no assumed automatic advertising integration |

The prebuilt financial scope is IAP and Stripe for the existing billing workflows. A wagering wallet, financial ledger, deposits, withdrawals, and wager settlement are not supplied by those integrations. Other financial functionality is implemented/integrated by the buyer or separately commissioned.

## Administration

Core includes an admin panel with these minimum controls:

- Player lookup and account blocking/unblocking.
- Active match and lobby visibility.
- Supported runtime settings.
- Push notification and in-app announcement management.
- Player-support request handling and operator replies.

Full Platform includes those controls plus the complete console for its additional modules, including social moderation, persistent rooms, progression/events, history/replay support, billing, and product analytics. Exact actions, permissions, and release screens will appear in the release manifest; source endpoint names alone are not proof that every operator action has a shipped UI.

## Features outside the confirmed first-release manifest

The reference source also contains platform game-service integrations, hosted-table workflows, email campaign tools, campaign links, shuffle-verification surfaces, and game-specific guidance/hints. Their reusable scope and package placement still need confirmation. They are not additional included-module promises from this catalog.

The visual client-wide design-system configurator is a later-release direction. The first release uses direct component/theme/asset customization and accompanying guidance.

A voice waitlist is not voice chat. Shop/inventory-related source is not a promised complete virtual economy. No arbitrary-language support, capacity figure, or provider certification follows from source presence alone.

## Your game and Jogada's implementation

Jogada's Buraco rules, game screens, offline engine/bots, replay screens, challenge content, and branding are excluded. Buyers may implement their own Buraco game within their license scope. Poker and blackjack are buyer-built examples, not included engines.

The tutorial is a small original teaching game with a separate integration walkthrough. Its exact rules/name and approved visual assets remain to be defined. It must be implemented and validated before being described as an available developer-release asset.

See the [architecture](architecture.md), [recorded recovery evaluation](recovery-evaluation.md), and [licensing summary](licensing-and-access.md) for technical evidence and commercial boundaries.
