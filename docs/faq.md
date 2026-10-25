# Frequently asked questions

[Back to the overview](../README.md) · [Feature catalog](features.md) · [Licensing](licensing-and-access.md)

## Who is this for?

Independent developers and small startup/studio teams starting a card or turn-based game with Flutter and Java. Studio/Agency agreements can cover broader client deliveries. Existing-game migrations are assessed separately.

## Can I download the developer package now?

The commercial distribution is being prepared. This repository contains documentation and evaluation media. Release contents, availability, and source access are confirmed before purchase; there is no immediate-access checkout.

## Is the reference game live?

Yes. [Jogada](https://jogada.com.br/) is the live Buraco product behind the infrastructure. [Play the web version](https://app.jogada.com.br/) to see its player experience. It is a production service, not a public load-test environment.

## Am I buying a copy of Jogada's Buraco game?

No. Its game rules, gameplay screens, offline engine/bots, replay/challenge content, and branding are excluded. The product is the reusable framework and package modules, with a small original card-game tutorial. You may implement your own Buraco game within your project's license scope.

## What do I build myself?

Your Java game-rules plugin, Flutter game screens/interactions, branding, provider/deployment configuration, and a separate Dart rules engine/bots if you want offline play. The licensed framework supplies the runtime, client foundation, platform modules, integration guidance, and tooling. Custom implementation can be commissioned separately.

## Does Core include Terraform and an admin panel?

Yes. Both packages include Terraform, Docker tooling, and a usable admin panel. Core's panel covers player lookup/blocking, active match/lobby visibility, supported runtime settings, push/announcement management, and player-support replies. Full Platform adds the complete console for its additional modules.

## What does Full Platform add?

Social/chat, persistent-room, achievement/ranking/event, daily-challenge, match history/replay, Stripe/IAP billing, product analytics, and complete admin capabilities. Game-specific objectives, scoring, replay rendering, and provider configuration still require adaptation. See the [feature matrix](features.md).

## Does one license cover poker and blackjack in the same app?

They are distinct games and need rights for both. A standard license covers one game and its variations for one branded product/customer project, including supported platform builds. Multiple customer deliveries need separate licenses or an agreed Studio/Agency scope.

## Are there usage, royalty, or developer-seat fees?

The standard software licenses are royalty-free, with no per-player, per-match, per-instance, or per-developer charges. Employees and contractors working on the licensed project may use and modify its delivered source. The buyer pays cloud and external-provider costs.

## Can I upgrade from Core to Full Platform?

Yes. The upgrade fee is US$499 for the same licensed game/project, with a fresh 12 months of Full Platform updates/repository access and a fresh 30 days of email installation help for the added modules. The first-reply target remains two business days. The standalone Full Platform license remains US$999.

## What happens after the first year?

The initial purchase includes 12 months of repository access and updates. Optional renewal is US$149 Core / US$299 Full Platform for another 12 months. You can continue operating and modifying delivered versions within the licensed scope without renewal.

## What support is included?

Thirty days of written installation help after purchase by email at [contato@jogada.com.br](mailto:contato@jogada.com.br), with a first-reply target of two business days. That target describes the initial response, not the resolution time. Live calls, guided deployment, custom coding, and migrations are separately scoped. The product's in-app support module lets your own team answer your players. No contractual support or production-operations SLA is advertised.

## Does a hot deployment keep every WebSocket open?

A socket belongs to its worker. Affected clients reconnect when that worker retires. Compatible rollouts are designed to preserve service availability and active match state. State, rules, and protocol compatibility remain requirements. Read the [failure model](architecture.md).

## Does Redis Pub/Sub store missed messages?

It provides live fan-out, not durable replay. Clients connect to Java over STOMP and recover by reconciling with authoritative state. Redis remains internal.

## Is the entire client free of game logic?

Online game-rule authority is in Java. Flutter still contains presentation, interaction, transport, and recovery logic. Standalone offline play uses a separate Dart game engine and bots. The offline implementation does not become authoritative online match state.

## What does the tutorial show?

A small original card game: private hands, deck state, turn validation, a Java plugin, Flutter screens, and separate offline Dart rules/simple bot. The first guide starts the Docker backend and Flutter example, plays an online match between two clients, tries the offline bot, and changes one rule. AWS/Terraform follows separately. The tutorial is a release requirement and is not yet available.

## Can I customize the visual design?

Use the reusable components/animations, replaceable themes/assets, approved starter artwork/editable files, and asset workflow guidance. A client-wide visual design-system configurator is a later-release direction. Custom graphic-design services are not offered.

## Is this a turnkey real-money gaming platform?

The prebuilt payment scope is Stripe and native IAP for the existing billing workflows in Full Platform. Other financial functionality is buyer-implemented or separately commissioned. These integrations do not supply a wagering wallet, ledger, deposits, withdrawals, or wager settlement.

## Do I need AWS for local or offline play?

The backend development harness uses local Docker services. Offline bot play does not need a multiplayer server. Initial dependency/image downloads and configured external integrations have their own network requirements. See [local development](local-development.md).

## Can I use another cloud or hire you to operate production?

The reference Terraform deployment targets the operator's AWS account. Another cloud requires adaptation. Buyers operate production by default; ongoing operations may be considered individually, and no standard managed-hosting plan is offered.

## What has actually been evaluated?

One isolated local run used two real Flutter web clients and multiple Java workers to verify recovery after a worker stop and a separate client WebSocket interruption. The [report](recovery-evaluation.md) records source versions, configuration, observations, and limits. It does not establish AWS rollout, workload capacity, native-platform, billing, or new plugin/tutorial acceptance.

## Is this open source?

This is a commercial source distribution with a public showcase. The public documentation/media are governed by [LICENSE.md](../LICENSE.md); commercial software rights come from a separate agreement. No open-source core is currently offered.
