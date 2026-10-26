# Technical evaluation guide

[Back to the overview](../README.md)

Evaluate behavior in a local harness or an explicitly designated staging
environment. The [live game](https://app.jogada.com.br/) is available for normal
play; it is not a load-testing or fault-injection target.

This guide is an evaluation procedure. A [published local recovery report](recovery-evaluation.md)
records worker loss and client WebSocket interruption for identified source versions.
It does not establish that every exercise below was run against a packaged commercial
release. Ask for release-specific evidence when evaluating a purchase.

## Record the environment

For each run, record the server and client versions, worker count, client
platforms, relevant configuration, and the failure or rollout being exercised.
Use test accounts and redact player identifiers and credentials before sharing
recordings or logs.

## 1. Cross-worker delivery

Join the same match from two independent clients. Confirm through local or staging
instrumentation that their sockets are attached to different Java workers; two
browser windows alone do not prove this. Submit legal moves from each client and
confirm both views converge on the server's current state.

This exercises the shared state and broadcast backplane rather than only one
worker's local broker.

## 2. Worker replacement during a match

Stop the worker serving one client while leaving healthy capacity, Redis, and
PostgreSQL available. Confirm the client re-authenticates, restores subscriptions,
and resumes the same match through a surviving worker.

Distinguish an acknowledged, committed move from an unacknowledged send. A network
failure during a send can leave the client uncertain whether it committed; inspect
the recovered state and deduplication behavior rather than assuming delivery.

Check match identity, the recovered hand, current turn, scoring, and absence of
duplicate actions. Record the visible reconnect interval without treating one
measurement as a universal recovery-time guarantee.

## 3. Client network interruption

Interrupt one client's connectivity, then restore it within the configured
session and seat-grace window. Check that state converges, controls recover,
subscriptions are restored, and stale frames do not overwrite newer state.

Repeat outside that window and confirm the application explains an expired seat
or ended match. A reconnect feature should not imply indefinite seat retention.

## 4. Compatible rolling deployment

Keep a match active while a compatible server revision is rolled out with
overlapping healthy capacity. Confirm both availability for new connections and
continuity for existing matches. Check that the client handles worker replacement
without inventing a new game or accepting stale state.

List the versions tested. A successful compatible rollout is not evidence that an
arbitrary rules or state-schema change can be deployed the same way.

## 5. Offline and billing paths

Test standalone bot play without a multiplayer server. Check the distinction
between offline results and online authoritative state.

Evaluate checkout, purchase restoration, and entitlement behavior using your
provider test environments and the intended platform builds. Local gameplay
success is not a substitute for exercising billing.

## Evidence to request

Include [observability and analytics checks](observability-and-analytics.md#release-evaluation-checks) in the evaluation: a controlled error with the correct release/environment, representative product events, operational metric/alert visibility, and telemetry suppression/failure behavior. Configure test-only destinations; avoid sending evaluation traffic to the reference game's provider projects.

| Evidence | What it should establish |
| --- | --- |
| Live product | The player experience in an operating game |
| Worker replacement recording | A known client moves to another worker and resumes its valid match |
| Network interruption recording | Current server state is recovered after a client reconnects |
| Compatible rollout record | Specific server/client versions and preserved match continuity |
| Fresh-workspace installation | A developer can run the licensed release using only its documented prerequisites |
| Workload benchmark | Versions, infrastructure, concurrent connections and matches, action rate, latency distribution, errors, duration, and methodology |

The [local report and recording](recovery-evaluation.md) provide functional recovery
evidence with source versions and observed intervals. No workload benchmark or AWS
rolling deployment record is published here. Numerical guarantees require their
own methodology and agreement.
