# SF-08 — Runtime Foundation

- **Status:** Proposed
- **Version:** 1.0
- **Owner:** Software Architecture
- **Related:** SF-06, SF-07, SF-09, SF-10

## 1. Purpose

Define reusable runtime responsibilities and boundaries for lifecycle, storage, networking, IPC and background work without pretending that every OS has identical semantics.

## 2. Runtime module boundaries

| Module | Responsibility | Must not own |
|---|---|---|
| Lifecycle | Start/stop/readiness and shutdown coordination | Product business rules |
| Configuration | Validated configuration snapshots | Secret vault implementation |
| Logging | Structured event emission and routing | Product analytics policy |
| Diagnostics | Health snapshot and support bundle orchestration | Arbitrary user-file collection |
| Storage | App data/cache/temp path policy and persistence adapters | Product domain schema |
| Network | Shared connection, timeout and retry primitives | Product protocol semantics |
| IPC | Authenticated local process communication | Unrelated business authorization |
| Scheduler | Background tasks, cancellation and deadlines | Unbounded work queues |
| Platform adapter | OS-specific implementation | Redefinition of common contract |

Projects may combine modules when scale is small, but ownership and interfaces should remain understandable.

## 3. Lifecycle contract

A long-running application/service SHOULD define:
- initialization order and dependency readiness;
- startup timeout and failure behavior;
- readiness and liveness;
- graceful shutdown and deadline;
- cancellation propagation;
- restart policy;
- recovery after abnormal termination.

Services should be supervised by the native platform manager where available (e.g. Windows Service Control Manager, systemd, launchd). Mobile and web applications must respect their host lifecycle and background-execution limits.

## 4. Storage contract

Separate:
- immutable application binaries/resources;
- user data;
- machine/device state;
- configuration;
- cache;
- temporary files;
- logs and diagnostics.

Define ownership, permissions, backup, retention and cleanup for each. Cache MUST be safely disposable. User data MUST NOT be removed during ordinary upgrade/uninstall unless explicitly requested and documented.

## 5. Network contract

Common networking primitives SHOULD standardize:
- connection and operation timeouts;
- cancellation;
- bounded retry with exponential backoff and jitter;
- idempotency rules;
- proxy configuration;
- TLS verification;
- error classification;
- observability and correlation.

Retry policy MUST account for operation idempotency. Do not automatically retry non-idempotent operations without an idempotency key or equivalent protection.

## 6. IPC contract

Local IPC MUST define:
- transport and endpoint discovery;
- message framing and schema version;
- authentication/authorization;
- request correlation and timeout;
- payload limits;
- backpressure and queue limits;
- shutdown behavior;
- compatibility policy.

Never assume that local IPC is trusted merely because it is local.

## 7. Background work

Tasks SHOULD support cancellation, deadlines, bounded concurrency and explicit ownership. Real-time/media paths require separate latency and resource budgets; generic task queues MUST NOT introduce unpredictable blocking into time-sensitive pipelines.

## 8. Platform adapter rules

A common interface defines semantics, not a lowest-common-denominator implementation. Each adapter documents:
- supported OS/runtime versions;
- unavailable capabilities;
- permission requirements;
- lifecycle differences;
- thread/process constraints;
- failure mapping;
- tests proving contract conformance.

Avoid exposing platform-specific types in shared business APIs unless the platform is intentionally part of the public contract.

## 9. Acceptance checklist

- [ ] Lifecycle and shutdown behavior are defined.
- [ ] Storage classes and ownership are explicit.
- [ ] Network timeouts/retries respect idempotency.
- [ ] IPC has authentication and bounded resources.
- [ ] Platform adapters document capability gaps.
- [ ] Critical runtime paths have measurable resource/latency budgets.
