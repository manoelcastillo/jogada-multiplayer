# Technology stack and operations

[Back to the overview](../README.md) · [Architecture](architecture.md) · [Core/Full features](features.md)

This inventory describes the reference system's implementation and infrastructure definitions, inspected on **25 October 2026**. It complements the product feature catalog with the details relevant to operating, extending, and evaluating the software.

Source snapshots: server `0fc65815aac2f0c38f9349c3705b62fa1d49d360` and client `9647660a89b2d813981837460f3cee0607950eb2`, also identified in the [recovery evaluation](recovery-evaluation.md). Source/configuration inspection establishes the characteristics below; it does not establish that every optional service is enabled in the live environment or validated in the commercial package. That package is being prepared, and its release manifest will identify supported versions, delivered modules, and acceptance evidence.

## Versions and build inputs

| Component | Inspected reference | Source of the version |
| --- | --- | --- |
| Backend Java | **21** | Maven compiler properties, JDK/JRE Docker stages, and backend CI |
| Spring Boot | **3.5.16** | Maven parent in `pom.xml` |
| Flutter | **3.47.2** | `FLUTTER_VERSION` in the mobile build workflow |
| Dart | SDK constraint **`^3.13.2`** | Client `pubspec.yaml`; this is a constraint, not a separately measured runtime version |
| Riverpod | **2.6.1** resolved; **`^2.5.1`** declared | Client `pubspec.lock` and `pubspec.yaml` |
| PostgreSQL | RDS engine **16.14**; local image **`postgres:16-alpine`** | Data-store Terraform module and Docker Compose |
| Redis | ElastiCache engine **7.1**; local image **`redis:7-alpine`** | Data-store Terraform module and Docker Compose |
| Flyway, Hibernate, HikariCP | Spring Boot dependency management | Maven dependencies/BOM; exact resolved versions belong in the release dependency manifest |

Version numbers describe this snapshot, not a promise that every future package uses those exact pins. Broad image tags can resolve to different patch builds. The licensed release must record the actual artifacts used for acceptance.

## Backend execution and persistence

| Characteristic | Implementation and purpose |
| --- | --- |
| Java virtual threads | Spring virtual-thread execution and embedded Tomcat virtual-thread handling are enabled. The backend retains a conventional Spring MVC/service/repository structure. Database connection pools, CPU work, and downstream capacity still constrain throughput. |
| Server-authoritative online play | Java validates player intent and controls committed match transitions. Flutter maintains presentation, animation, selection, and recovery state. Standalone offline rules and bots execute separately in Dart. |
| Redis live-state authority | Live match state resides outside worker JVMs. Atomic Lua compare-and-set writes check a monotonic version before updating state; stale operations must re-read and re-evaluate. |
| Move deduplication | Redis-backed move-ID guards cover repeated commands across workers within their configured retention window. They depend on the request carrying an ID; they are not a blanket exactly-once delivery guarantee. |
| Timer/worker coordination | Redis leases establish ownership and suppress stale timer/driver actions across workers. This is not a general durable scheduler or proof that every pending timer is recovered after owner death. |
| Redis Pub/Sub backplane | Workers publish broadcasts through the backend backplane and deliver them to their connected STOMP subscribers. Pub/Sub is transient fan-out; reconnecting clients recover authoritative state rather than replaying a durable queue. |
| PostgreSQL application persistence | JPA/Hibernate manages persistent application records and the reference product's stored game records. HikariCP pools connections; open-session-in-view is disabled. |
| Flyway migrations | Versioned SQL migrations own schema evolution. Hibernate uses `ddl-auto=validate`, so startup checks the schema instead of changing it implicitly. Production configuration separates migration credentials from the application database role. |

Compatible application changes, state schemas, plugins, and database migrations must be considered together during a rollout. Flyway migration execution and the ECS circuit breaker do not automatically reverse an applied schema change.

See the [architecture and failure model](architecture.md) for state ownership, client subscriptions, recovery, and game-plugin boundaries.

## AWS and infrastructure as code

| Component | Implemented infrastructure characteristic |
| --- | --- |
| ECS Fargate | Containerized Java workers with a configurable desired count. The inspected module defines rolling deployment capacity and a deployment circuit breaker with rollback when a revision cannot become healthy. |
| Application Load Balancer | HTTPS termination, HTTP-to-HTTPS redirect, WebSocket routing, readiness-based target health checks, cookie stickiness, and target deregistration/draining. Existing sockets remain attached to their workers until disconnected. |
| VPC and security groups | Network definitions separate application access from data-store access. RDS and ElastiCache use private subnets; the application security group admits its service port from the ALB security group. |
| RDS PostgreSQL | Encrypted storage, automated backups, storage growth headroom, configurable Multi-AZ/deletion protection, and an RDS-managed master password stored in Secrets Manager. Restore procedures and workload sizing remain operator responsibilities. |
| ElastiCache Redis | Encrypted storage and configurable node count; the module enables automatic failover and Multi-AZ when replicas are configured. Application-to-Redis TLS is not enabled in the inspected module and needs review for the operator's deployment requirements. |
| ECR | Immutable image tags, scan-on-push configuration, and an image-retention policy. Backend build metadata can identify the source revision serving requests. |
| Route 53 and ACM | DNS records and managed certificate configuration for the API and configured web-hosting domains. |
| S3 and CloudFront | Infrastructure modules for Flutter web/PWA hosting and avatar delivery, including private bucket access controls and CloudFront origin access control. Buyer domains, content, and accounts need configuration. |
| Parameter Store and Secrets Manager | Task execution roles read configured secrets/parameters; placeholder values and operator provisioning are separate from application source. The release still needs neutral configuration and a full sanitization review. |
| Terraform remote state | Bootstrap definitions provide a versioned, encrypted S3 state bucket and DynamoDB locking. Operators own the backend configuration and access policy. |
| GitHub OIDC role definitions | Terraform defines repository/ref/environment-scoped AWS trust for deployment identities. This provides an alternative to long-lived AWS keys for automation; it does not imply that every deployment script already uses that path. |

Worker count can be changed independently of game count. The inspected Terraform does **not** define a demand-driven ECS autoscaling policy; scaling metrics, thresholds, limits, and validation belong to the operator/release configuration. This inventory describes code and configuration, not a live AWS account audit.

## Health, logs, metrics, and alerts

| Capability | Implementation and operational value |
| --- | --- |
| Separate liveness and readiness | Actuator liveness reports process state. Readiness includes application state, PostgreSQL, Redis, and the STOMP broker; the Redis contributor has a bounded transient-outage tolerance. Readiness is the configured ALB target check. |
| Container health check | The Java Docker image defines a health check and startup allowance. The ECS service also defines a startup health-check grace period. These settings need tuning for the operator's build and environment. |
| CloudWatch logs | ECS uses the `awslogs` driver. The production Logback profile writes structured JSON to stdout, with correlation context propagated through HTTP and STOMP paths. |
| Deployed build identity | Actuator build information includes build metadata and a source commit when the image is built with its commit input, helping operators identify the serving revision. |
| CloudWatch alarms and SNS | Terraform defines alarms for healthy ALB targets, target errors/latency, ECS CPU/memory, RDS CPU/storage/connections, Redis CPU/evictions, and SMS spend. Alerts route to an operator-configured SNS email subscription. |
| Micrometer and Prometheus | JVM/application instrumentation is exposed through protected Actuator metrics and Prometheus endpoints. The reference includes matchmaking timers and operational game/queue measurements. |
| Local Grafana dashboards | Docker Compose and provisioned Prometheus/Grafana configuration provide a local monitoring path. They are distinct from a managed production monitoring service. |
| Optional access logs | ALB and web-CDN modules include configurable access-log delivery/retention. Operators choose enablement, destinations, access controls, and retention for their environment. |
| Client diagnostics | The Flutter crash-reporting abstraction includes a Sentry adapter and scrubbing of recognized sensitive values and breadcrumb data. Buyer-owned destinations and privacy settings must be configured and verified. |

Operational health/metrics are separate from Full Platform's player activity, retention, and acquisition reports. CloudWatch and local dashboards do not make product analytics a Core entitlement.

## API and security controls

| Characteristic | Reference behavior |
| --- | --- |
| Spring Security | Role-gated operator endpoints and views, configured origin controls, and security-header handling. Permissions still need validation after package extraction and customization. |
| OpenAPI and Swagger UI | Springdoc generates REST API/schema documentation. Documentation routes are protected by operator roles rather than exposed as a public endpoint catalog. |
| Distributed rate limiting | Redis-backed atomic counters/expiry support REST and STOMP limits across workers. Domain-specific budgets and abuse controls need review for the buyer's game and providers. |
| Sensitive operational endpoints | Metrics/Prometheus require admin authorization; health/build information has a different exposure policy for routing and diagnosis. Operators must review endpoint exposure in their deployment. |
| Runtime and client-version policy | The reference has explicit runtime-setting allowlists and client-version policy controls. Supported settings have defined scopes; arbitrary changes during active games are not implied. |

These are implemented controls, not a security certification. Private-state isolation, authorization, provider ownership, secrets handling, and dependency posture must be checked against the exact delivered release.

## Flutter client characteristics

| Characteristic | Reference behavior |
| --- | --- |
| Riverpod state boundaries | Views use `Notifier` / `NotifierProvider` controllers. Repositories isolate domain-facing operations from Dio REST and WebSocket/STOMP transport. |
| Connection lifecycle | STOMP heartbeats, reconnect backoff with jitter, foreground/liveness handling, and application-level resubscription/state recovery address interrupted sessions. Browser backgrounding and native lifecycle paths still need release-specific acceptance. |
| Bounded HTTP retries | The Dio retry interceptor honors HTTP 429 `Retry-After`, with an attempt limit, jitter, and endpoint exclusions. It is not an indiscriminate retry policy for all errors or every purchase action. |
| Secure-storage integration | The client uses a secure-storage abstraction/package for applicable stored credentials; capabilities and browser behavior vary by platform and need deployment review. |
| Localization pipeline | ARB catalogs and generated Flutter localizations support language selection. The reference contains Portuguese, English, and Spanish catalogs; buyers must adapt game content and verify translations for their supported release. |
| Presentation customization | Reusable theme/component/animation boundaries allow asset and visual replacement. The existing card-animation pipeline serializes event-driven animation; the commercial extraction must replace Buraco-specific presentation. |
| Diagnostics and analytics adapters | Crash reporting and analytics use adapter interfaces. Product analytics is Full Platform scope; buyer-owned provider accounts, destinations, event definitions, and privacy controls require setup. |

The new tutorial, neutral graphics framework, and separately implemented Dart tutorial engine/bot remain developer-release work. A visual design-system configurator remains a later roadmap item. This inventory does not add provider integrations or reference-game implementations to the agreed package scope.

## Build and engineering tooling

| Tooling | Inspected characteristic |
| --- | --- |
| Maven Wrapper and multi-stage Docker | The backend image builds its JAR inside a JDK stage, caches dependencies through BuildKit, and runs a separate Java 21 JRE stage. Initial builds require dependency/image downloads. |
| Docker Compose and Traefik | Local PostgreSQL/Redis and multi-worker routing support development and recovery evaluation. Local provider substitutes and neutral examples still need packaging. |
| GitHub Actions backend CI | Maven `verify` runs tests and a JaCoCo line-coverage gate. The inspected configured floor is **80%**; this is a gate, not a claim about current measured coverage. |
| Testcontainers | PostgreSQL-backed integration tests exercise real database behavior for selected migration, reporting, and billing/concurrency cases. This does not mean the whole application is covered by end-to-end tests. |
| Dependency checks | Backend CI uses Trivy to fail on configured fixable high/critical findings, subject to documented suppressions. Dependabot configuration supports dependency maintenance. Neither establishes an absence of vulnerabilities. |
| Flutter checks and mobile builds | Client tooling includes analysis, unit/widget and selected browser tests. The mobile GitHub Actions workflow defines Android release and iOS build jobs with a pinned Flutter SDK; signing and store submission are separate. |

## Delivery and validation boundary

Core and Full both include the accepted runtime/client foundation, Docker tooling, Terraform, and documentation. Full adds the modules in the [feature catalog](features.md); this page does not move reference-only growth/game-service features into either package.

Source inspection for this page did not run cloud deployments, restore drills, native builds, load tests, or provider transactions. The [recorded recovery evaluation](recovery-evaluation.md) supplies narrower runtime evidence with its own versions and limits. Commercial acceptance must still verify the extracted plugin contract, original tutorial, clean installation, Core/Full boundaries, native/offline paths, provider workflows, and compatible AWS rollout.
