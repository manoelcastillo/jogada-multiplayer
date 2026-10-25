# Local onboarding and reference workspace preview

[Back to the overview](../README.md)

This showcase does not include a runnable server, client, Docker environment, or
Terraform implementation. The commands below describe the existing source
workspace used by Jogada. Access is discussed in [Licensing and access](licensing-and-access.md).

The commercial release's final packaging and release-specific setup guide will
be provided with that release. No private clone URL or unimplemented wrapper is
presented here as an available product.

## Intended first onboarding milestone

The developer-release walkthrough will use a small original card game and proceeds in this order:

1. Start the local Docker backend and Flutter example.
2. Play an online match between two clients.
3. Try standalone offline play against the simple bot.
4. Modify one example rule, with the walkthrough explaining the separate Java online and Dart offline implementations.

AWS deployment through Terraform follows in a separate guide. This onboarding sequence is a release requirement, not an available tutorial, a measured setup duration, or an included hands-on deployment service. The developer release will supply its exact paths, commands, supported versions, and configuration.

The commands below describe the internal reference workspace used for engineering evaluation. Jogada's Buraco implementation is excluded from commercial delivery. They are not a promise that cloning this public showcase installs the application or the new tutorial.

## Reference-workspace prerequisites

- Access to the matching server and client source versions.
- Docker Engine or Docker Desktop, with a recent Docker Compose v2 supporting the `!override` merge tag.
- The Flutter SDK version required by that source release, plus Chrome for the web development client.
- Free local ports for the reverse proxy and development client.
- Development configuration for any optional external integration being exercised.

Docker builds the Java application in the container. Initial builds and package
resolution download dependencies; cache them before testing without internet.

## Existing source layout

```text
source-workspace/
├── jogada_server/
│   ├── run-local.sh
│   ├── docker-compose.yml
│   ├── docker-compose.cluster.yml
│   ├── src/
│   └── infra/terraform/
└── jogada_client/
    ├── pubspec.yaml
    ├── pubspec.lock
    ├── lib/
    ├── android/
    ├── ios/
    └── web/
```

This is the source layout, not the file tree of this showcase repository.

## Start the backend on macOS or Linux

From the parent of the server directory:

```bash
cd jogada_server
./run-local.sh 3
```

The launcher builds and starts three Java workers, Redis, PostgreSQL, and Traefik.
Clients use `http://localhost:8080` and `ws://localhost:8080/ws` through the proxy.
The launcher follows logs after startup; interrupting the log tail leaves the
containers running.

## Start the backend from PowerShell

The same Compose files can be used directly from a suitable Docker environment:

```powershell
Set-Location jogada_server
$env:JOGADA_GAME_STATE_BACKEND = 'redis'
docker compose -f docker-compose.yml -f docker-compose.cluster.yml up -d --build --scale jogada_server=3
```

This is a Compose invocation, not a claim that this showcase ships a PowerShell
launcher. Docker Desktop must be configured to run the required Linux containers
and support the local proxy's Docker socket mount.

## Start the Flutter web client

In a second terminal, from the parent of the client directory:

```bash
cd jogada_client
flutter pub get
flutter run -d chrome --dart-define=JOGADA_HTTP=http://localhost:8080 --dart-define=JOGADA_STOMP=ws://localhost:8080/ws
```

Use two independent browser sessions for a two-player evaluation. A native device
or emulator may need a reachable host address instead of `localhost`. Native
builds also require the corresponding Android or iOS toolchain and signing setup.

## Inspect and stop the environment

From the server directory:

```bash
./run-local.sh logs
./run-local.sh down
```

`down` stops the application workers and local proxy while leaving Redis and
PostgreSQL running. For full service shutdown while keeping named data volumes:

```bash
./run-local.sh down-all
```

The equivalent full service shutdown from PowerShell is:

```powershell
docker compose -f docker-compose.yml -f docker-compose.cluster.yml down
```

## What local operation covers

The harness provides local data services and a proxy in place of AWS routing.
It supports development and the [recovery evaluation](evaluation.md).

Live Stripe, app-store purchase validation, authentication providers, and messaging
services have their own connectivity and account requirements. A locally running
gameplay stack does not imply that every external integration works offline.

Never apply worker-stop or fault-injection exercises to the live public game.
