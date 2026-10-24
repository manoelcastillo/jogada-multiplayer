# Frequently asked questions

[Back to the overview](../README.md)

## Is this repository the runnable product?

It contains the showcase and documentation. The application code, local harness,
and Terraform implementation are part of a separate commercial distribution
being prepared. See [licensing and access](licensing-and-access.md).

## Is the reference game live?

Yes. [Jogada](https://jogada.com.br/) is the live Buraco card-game product behind
this architecture. You can [open the web game](https://app.jogada.com.br/) for normal
play. Its production environment is not a public benchmark or testing sandbox.

## Is this open source or open core?

No open-source core is offered in this showcase. The original documentation and
diagrams have the rights notice in [LICENSE.md](../LICENSE.md). Commercial software
rights are provided by a separate agreement.

## What does server-authoritative mean here?

For online multiplayer, Java validates intent and owns match transitions and
scoring. The client renders the resulting player-visible state. Editing client
memory does not make a forged result authoritative. This boundary does not remove
every security concern: automation, account abuse, authorization, and information
exposure still require appropriate controls.

## Does the client really have no game logic?

Online rule authority is server-side. The client still has presentation,
interaction, transport, and recovery logic. Offline single-player additionally
uses a local Dart game engine and bots. Describing the entire client as having
zero business logic would obscure that distinction.

## Does a hot deployment keep every WebSocket open?

A socket belongs to its worker process. Affected clients reconnect when that
worker is replaced. Compatible rollouts are designed to preserve service
availability and active match state through that transition. Read the
[failure model](architecture.md) before interpreting zero downtime as a guarantee.

## Does Redis Pub/Sub store missed messages?

It does not provide durable replay. The client recovers by reconciling with
current authoritative state after reconnecting. STOMP is the client-facing
protocol; Redis stays inside the backend.

## Can I use this for another game?

Yes. The server follows a plugin architecture in which each game's
rules and engine are implemented as a plugin. Developers can add, remove, and
manage those game implementations while reusing the networking, shared state,
reconnection, and deployment foundation. Each game engine encapsulates validation,
scoring, and match transitions behind the shared plugin contract.

The live reference game is Buraco. Other games require their own rules, payloads,
client experience, and potentially scheduling changes.

## Can the backend run more than two Java workers?

The architecture uses shared state and a broadcast backplane so the worker pool
can contain more than two instances. Workers A, B, and N in the overview illustrate
that pool. Operators configure capacity and scaling policies for their workload;
adding workers still requires adequate shared-service capacity and validation.

## Can I host it myself or use another cloud?

The reference deployment is self-hosted in the operator's AWS account using
Fargate, ALB, Redis, PostgreSQL, and Terraform. Containers can be a starting point
for another deployment, but an equivalent Terraform deployment for another cloud
is not promised here.

## Does local or offline operation require AWS?

Standalone bot play does not need a multiplayer server. The Docker harness uses
local backend services. Initial dependency downloads and configured external
integrations have their own network requirements. See [local development](local-development.md).

## What capacity and support are guaranteed?

No capacity, latency, recovery-time, uptime, or support SLA is published in this
showcase. Evaluate the target workload using the [evaluation guide](evaluation.md).
Any contractual support commitment must appear in the commercial agreement.

## How much does a license cost?

Pricing and final package scope are provided directly. Email
[contato@jogada.com.br](mailto:contato@jogada.com.br?subject=Jogada%20Multiplayer%20-%20Commercial%20licensing)
with your game type, platforms, team size, and timeline.
