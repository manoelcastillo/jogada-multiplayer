# Recovery evaluation: two workers, one match

[Back to the overview](../README.md) · [Evaluation procedure](evaluation.md)

On 25 October 2026, an isolated local run exercised two real Flutter web clients in one private match, initially attached to different Java workers. One worker was abruptly stopped. The affected client reconnected through the surviving worker and recovered the same match and player-visible state. The other client's socket stayed open.

The same run then closed Client B's WebSocket and observed automatic reconnection and state recovery. Both scenarios passed the recorded assertions.

[![Two real Flutter clients in the recovery evaluation](../assets/demo/recovery-poster.png)](https://manoelcastillo.github.io/jogada-multiplayer/)

[Watch the recording](https://manoelcastillo.github.io/jogada-multiplayer/) · [Machine-readable results](../assets/demo/evaluation.json) · [Why the match survives](engineering/match-continuity.md)

## Environment and source versions

| Item | Configuration |
| --- | --- |
| Server source | `0fc65815aac2f0c38f9349c3705b62fa1d49d360` |
| Client source | `9647660a89b2d813981837460f3cee0607950eb2` |
| Backend | Two Java containers, Traefik, Redis, PostgreSQL; isolated Docker Compose project and data volumes |
| Clients | Two Flutter web instances in a fresh Chrome context, with separate development slot storage |
| Match | Two synthetic test players; private two-player match; no-pressure mode, with turn timers disabled |
| Rules | Normal server-side validation enabled; permissive development plays disabled |
| Authentication | Dev-only synthetic session endpoint, followed by normal authenticated gameplay; OTP delivery was not exercised |
| Isolation | Separate ports, networks, volumes, source extractions, and container image; no production traffic or existing development data used |

The source was extracted from committed Git trees without history or ignored configuration. The backend image built, both workers became ready, and the Flutter web client compiled and started from that extraction. The host already had Docker, Flutter, Chrome, and dependency caches. This establishes a fresh-source local run, rather than an independent clean-machine installation of a packaged commercial release.

## Method

1. Establish both clients' authenticated STOMP sessions. Assign distinct valid proxy affinity cookies before their connections open.
2. Create and join a real private match. Submit a legal draw through the active client's existing authenticated socket and observe its server acknowledgement.
3. Request authoritative snapshots for both players. Record the selected state fields and each client's socket lifecycle.
4. Abruptly stop one Java container, leaving Redis, PostgreSQL, the proxy, and the other worker running.
5. Wait for the affected Flutter client to reconnect and receive a new authoritative snapshot. Compare it with that player's preceding snapshot.
6. Close Client B's current game WebSocket with application close code `4000`. Wait for the client's automatic reconnect and snapshot reconciliation, then compare state again.

The application source was unchanged. External test instrumentation observed sockets and messages and submitted the setup/gameplay actions through the existing connections. This was an automated recovery scenario using the real client, rather than a manual test of every UI control.

## Observed results

| Check | Result |
| --- | --- |
| Both clients initially in the same match | Passed |
| Legal draw acknowledged by the server | Passed |
| Exactly one client socket replaced after worker stop | Passed; Client B reconnected and Client A's socket remained open |
| Match identity preserved after worker stop | Passed |
| Selected player-visible state preserved after worker stop | Passed for both player views |
| Same match and selected state after Client B transport interruption | Passed |
| Worker-stop request to recovered snapshot | 1,116 ms observed in this run |
| Client socket close to recovered snapshot | 2,222 ms observed in this run |

The snapshot comparison covered match identity and public commitment, player identities and visible hands, current player, game phase, round, scores, match points, melds, deck count, discard pile, foot counts, and no-pressure setting. It excluded connection/presence metadata and presentation state. Per-player before/after hashes for the worker scenario are included in the [result file](../assets/demo/evaluation.json).

The scores and melds were at their initial values in this scenario; equality does not establish recovery of every scoring or round-transition path. Recovery timing starts at the test action and ends at reception of the authoritative snapshot. It is not a measurement of first rendered frame or a contractual bound.

## Interpretation and limits

This is one functional local run. It supports match continuity across a Java worker loss and reconnection in the tested versions and configuration. It is not a workload benchmark, a production availability measurement, or an AWS deployment test.

The client interruption explicitly closes a WebSocket. It does not simulate an operating-system network outage, packet loss, a mobile network handoff, or reconnecting outside the seat-grace window. No Redis/PostgreSQL failure, timer failover, native billing, offline mode, or plugin integration acceptance test was performed here.

Compatible AWS rolling deployment still needs its own evidence, identifying both old and new revisions and checking state/protocol compatibility. The commercial package needs its own installation and release acceptance checks before delivery.

The approximately 18-second recording trims startup and warmup. The remaining interval is continuous at normal speed, without audio. Visible players are synthetic local test accounts; the recording contains no production accounts or credentials.
