# Keeping multiplayer card matches alive across Java worker replacements

[Back to the overview](../../README.md) · [Architecture](../architecture.md) · [Recorded evaluation](../recovery-evaluation.md)

A match and a WebSocket have different lifetimes. A socket belongs to a process. A multiplayer match needs to remain valid when that process is replaced and when a player briefly loses their connection.

Jogada separates those lifetimes: Java owns online rule execution, Redis holds live match state outside the workers, and Flutter recovers the player view from current server state after reconnecting.

## The boundary that matters

The client submits intent: draw, discard, lay cards, or another game action. Java validates that intent against the current match and rules. A successful transition becomes shared authoritative state. A client-side animation or changed memory value cannot commit that transition.

An application worker still owns connections and performs work. Here, stateless describes the ownership of authoritative live match data; it does not mean that a worker has no connection state or local resources.

Game engines and rules follow a plugin architecture. Each game's validation, scoring, and transitions sit behind the shared runtime contract, while networking and state recovery remain common infrastructure. A new game still needs its own rules and client experience. Modularity does not make an incompatible rule or state change safe to deploy mid-match.

## State recovery and live delivery serve different jobs

Redis Pub/Sub distributes live messages across Java workers. Each worker can deliver the relevant STOMP messages to its connected clients. Clients connect to Java, never directly to Redis.

Pub/Sub does not provide durable replay. A client that disconnects can miss events, so reconnecting must do more than reopen its socket. It must restore subscriptions and reconcile with a fresh, player-scoped authoritative snapshot.

That snapshot is also the answer when a connection fails around a move. An attempted send and a committed action are different observations. Recovery must inspect current server state and the action's acknowledgement/deduplication behavior rather than blindly repeat the animation or assume that the move failed.

Shared updates also require concurrency control. The runtime uses optimistic version checks and coordination so multiple workers do not simply overwrite one another's match transitions. Redis availability and adequate shared-service capacity remain operational requirements.

## What happens when a worker disappears

Consider two clients in the same match, each connected through a different Java worker:

1. Both views receive updates for the shared match through the backend broadcast path.
2. One worker stops. Its WebSocket closes; the surviving worker's connection can remain open.
3. The affected Flutter client reconnects through the proxy or load balancer to healthy capacity.
4. Session and seat checks determine whether the player may resume. The client restores its subscriptions and reconciles with current state.

The match does not need to be reconstructed from that client's last animation or from the stopped process's heap.

In the Flutter implementation, Riverpod controllers coordinate state through repositories and network adapters. Recovery belongs in that state flow. A screen should receive a reconciled game view instead of needing to implement its own transport lifecycle.

## The local evidence

The [25 October 2026 evaluation](../recovery-evaluation.md) used two real Flutter web clients, two Java containers, Redis, PostgreSQL, and a local proxy. Normal online rule validation was enabled. After an acknowledged draw, one worker was abruptly stopped.

Exactly one client socket was replaced. The same match and selected player-visible state were recovered; the other socket stayed open. The test then closed the affected client's WebSocket again and verified another automatic reconnect and reconciliation. The report includes source revisions, checks, observed intervals, a recording, and explicit exclusions.

The test used a private two-player, no-pressure match. It establishes that recovery path for those versions, not timer failover, every game transition, native mobile networking, or a capacity ceiling.

## Deployment is a compatibility problem as well

Externalizing state makes worker replacement possible. It does not make arbitrary deployments safe. During a rolling deployment, old and new workers may read and write the same match state and serve clients using the existing protocol.

State schemas, message formats, rule interpretation, and migration behavior need an explicit compatibility strategy. The load balancer also needs healthy replacement capacity. A client attached to a retiring worker may reconnect even while the service remains available.

The reference AWS deployment uses Fargate, an Application Load Balancer, and Terraform. The local worker-stop evaluation does not exercise that deployment path. A rollout claim needs a separate record with the actual old/new revisions and environment.

## Evaluate the adoption boundary

For a developer or studio, the useful questions are concrete: can a fresh source release start from documented prerequisites, can your engine integrate behind the plugin contract, does recovery preserve your game's valid state, and can your team operate the shared services?

The [evaluation guide](../evaluation.md) provides those checks. The [licensing page](../licensing-and-access.md) describes the separate commercial source distribution being prepared. The public repository contains the architecture and evidence; it is the starting point for evaluation and a technical conversation.
