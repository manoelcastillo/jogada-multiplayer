# Commercial licensing and private source access

[Back to the overview](../README.md) · [Compare package features](features.md)

The commercial developer distribution is being prepared for independent developers and small startup/studio teams starting a new game. Studio/Agency licensing also covers separately agreed client/portfolio scopes. This page summarizes the intended offer; the release contents, availability, and commercial agreement are confirmed before purchase.

## Packages and initial pricing

| Package | Initial license, USD | Optional additional 12 months of updates/access, USD |
| --- | ---: | ---: |
| Core | **$499** | **$149** |
| Full Platform | **$999** | **$299** |
| Studio / Agency | Scoped quote | Stated in the quote |

Core includes the multiplayer/runtime and Flutter foundation, accounts, lobbies/matchmaking/private matches, reconnection, push/announcements, player-support tools, a simpler admin panel, the original card-game tutorial, Docker tooling, Terraform, and documentation. Full Platform inherits Core and adds social, persistent-room, progression/event, history/replay, billing, product analytics, and complete admin modules.

See the [feature matrix](features.md) for integration work, exclusions, and items still under scope review. These initial package prices describe the intended first release; they are not an immediate-access checkout. Any additional services and order details are agreed before payment.

## Core to Full Platform upgrade

An existing Core license can be upgraded to Full Platform for **US$499**, covering the same licensed game/project. The upgrade starts a **fresh 12 months of Full Platform repository access and updates**. It does not add another game, brand, or customer project.

The upgrade also starts a **fresh 30 days of email installation help for the added Full Platform modules**, with the same **two-business-day first-reply target**. Custom implementation and hands-on deployment remain separately scoped. The standalone package prices remain US$499 Core / US$999 Full Platform; the chosen upgrade fee is US$499.

## Standard license unit

One standard license covers **one game and its variations for one branded product/customer project**.

- The same game's Android, iOS, and Web builds are included.
- Development, staging, and production deployments and elastic worker scaling are included within that project.
- Multiple variations of poker can belong to the poker game license.
- Poker and blackjack are separate games, including when delivered inside one app; obtain rights for both.
- Reusing the same game for another brand/customer project requires another standard license or an agreed broader scope.

The whole project team may use and modify the delivered source, including employees and contractors working on that project. There are no per-developer software fees. This does not grant unrelated-project reuse, unrestricted source resale, or automatic source-rights transfer to another entity.

## Runtime charging

Both standard packages are royalty-free. There are no Jogada software charges per player, match, worker instance, or developer. The buyer pays infrastructure and configured external-provider costs independently.

## Updates, access, and continued use

The purchase includes **12 months of private repository access and updates**. An optional renewal buys another 12 months at the stated package renewal price.

You may keep using and modifying versions already delivered within your licensed scope without renewing. An existing game does not require renewal to keep operating. Private update/repository access and continuing use of delivered code are separate entitlements.

No lifetime-update entitlement or additional custom implementation/support service is created by the renewal. Repository account onboarding and access administration are defined in the release's fulfillment process.

## Installation support and optional services

Both standard packages include **30 days of written installation help after purchase by email** at [contato@jogada.com.br](mailto:contato@jogada.com.br) for the documented setup. The first-reply target is **two business days**. This is an initial response target, not an issue-resolution deadline or a contractual SLA.

Live calls, guided deployment, new game plugins, Flutter screens, integrations, and existing-game migrations are separately scoped paid work. Availability, deliverables, fees, and any ownership/reuse terms are agreed for each engagement. Custom graphic-design services are excluded; the framework provides approved reusable assets and customization workflows within its package scope.

The player-support feature inside the product lets the buyer's staff answer their players. It does not mean Jogada staff operate the buyer's customer-support desk.

## Hosting and operation

Buyers operate production by default and supply AWS/provider accounts and configuration. Terraform is included in Core and Full Platform. The initial onboarding guide is local-first; AWS/Terraform deployment follows in its own guide.

Ongoing production operation may be considered individually according to time, scope, availability, and compensation. It is not a standard managed-hosting package or SLA.

## Included examples and excluded game implementation

The intended developer release includes a small original card-game tutorial, with a Java online rules plugin, Flutter screens, and a separate Dart offline implementation/simple bot. The tutorial is still to be built and validated; exact release contents are confirmed before purchase.

Jogada's own Buraco rules, screens, offline engine/bots, replay/challenge content, and branding are not distributed in either package or as a separately paid Buraco starter. Buyers may build their own Buraco implementation using the framework within their licensed project scope.

There is no catalog of bundled poker/blackjack engines. Game rules are implemented through the plugin architecture; buyers build their own game experience and offline logic where applicable.

## Request evaluation and source access

Email **[contato@jogada.com.br](mailto:contato@jogada.com.br?subject=Jogada%20Multiplayer%20-%20Developer%20licensing)** with:

- Your game type and target platforms.
- Team context and intended timeline.
- Whether the project is your own game or a client delivery.
- Package interest and evaluation requirements.

The intended fulfillment sequence is:

1. Confirm release readiness, package contents, project rights, access/update period, and services.
2. Complete the applicable agreement and payment process.
3. Supply the authorized GitHub account information through direct onboarding.
4. Accept access to the private distribution repository using the release's tested fulfillment process.
5. Follow the local card-game walkthrough, then the AWS/Terraform guide if deploying there.

No private repository URL, release date, automated checkout, or immediate-access promise is announced here. Keep confidential project and onboarding information in direct correspondence.

## Showcase rights

The public repository's original materials are governed by [LICENSE.md](../LICENSE.md). That notice grants no commercial implementation rights. This is a premium source distribution with a public showcase; no open-source core is currently offered.
