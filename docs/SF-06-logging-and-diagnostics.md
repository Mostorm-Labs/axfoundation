# SF-06 — Logging & Diagnostics

- **Status:** Proposed
- **Version:** 1.0
- **Owner:** Software Operations
- **Related:** SF-03, SF-05, SF-08, SF-09

## 1. Purpose

Make software behavior observable and customer issues diagnosable while protecting privacy and limiting operational overhead.

## 2. Structured log event

Applications SHOULD emit structured logs. A common event shape is:

```json
{
  "timestamp": "2026-11-10T10:00:00.000Z",
  "level": "ERROR",
  "product_id": "com.example.nearcast",
  "product_version": "2.4.1",
  "build_id": "20261110.1842",
  "platform": "windows",
  "module": "receiver",
  "event": "stream_connection_failed",
  "error_code": "NC-1001",
  "trace_id": "optional-correlation-id",
  "message": "Failed to establish stream",
  "context": { "transport": "airplay" }
}
```

Field names and severity semantics SHOULD remain stable across languages. Platform-native logs may be collected in addition to the common format.

## 3. Severity

| Level | Meaning |
|---|---|
| TRACE | Fine-grained execution detail; disabled by default in production |
| DEBUG | Diagnostic detail for developers/support |
| INFO | Expected lifecycle and business events |
| WARN | Recoverable abnormal condition or degraded behavior |
| ERROR | Operation failed; process may continue |
| FATAL | Critical failure; process/service cannot safely continue |

Do not log the same error repeatedly at every layer. Log at the point with useful context and propagate structured error information.

## 4. Required context

Where available, logs SHOULD include product/build identity, module, event, stable error code, timestamp and correlation/trace ID. Device identifiers MUST be pseudonymous or minimized in exported data unless operationally necessary and authorized.

Timestamps SHOULD be UTC and machine-readable. Monotonic clocks SHOULD be used for durations and ordering within a process.

## 5. Storage and retention

Each product MUST define:
- log location and access permissions;
- maximum file size and total storage budget;
- rotation policy;
- retention period;
- behavior when storage is full;
- export mechanism;
- dynamic log-level control if supported.

Logging MUST NOT exhaust disk space or block critical real-time paths. Sensitive values, tokens, passwords, private keys, personal data and session contents MUST NOT be logged.

## 6. Error taxonomy

Use stable, namespaced error codes. Example:
- `COMMON-1xxx`: configuration, storage, permissions and common runtime;
- `UPDATE-1xxx`: download, verification, apply and rollback;
- `DEVICE-1xxx`: hardware discovery and initialization;
- product-specific prefixes for domain failures.

Error codes MUST have documented meaning, retryability, user impact and diagnostic guidance. Do not reuse a code for a different failure.

## 7. Crash and health reporting

Where platform capabilities permit, capture:
- crash dumps/minidumps or OS crash reports;
- exception and stack trace;
- product/build identity;
- process uptime and restart count;
- relevant non-sensitive runtime state.

Health checks SHOULD distinguish liveness (process responds), readiness (can serve work) and dependency health (required services/devices are available). A process running is not proof that the product is healthy.

## 8. Diagnostic bundle

A support bundle SHOULD include, subject to consent and redaction:
- application logs;
- crash reports;
- software/build/package versions;
- OS, architecture and relevant hardware summary;
- update/install history;
- sanitized configuration;
- network diagnostics appropriate to the product;
- health/status snapshots.

The bundle MUST include a manifest describing collected files, time range and redaction status. Collection MUST avoid credentials, personal content and unrelated user files. User confirmation SHOULD be required before upload or external sharing.

## 9. Remote telemetry

Telemetry collection MUST have a defined purpose, data minimization, retention, access control and customer/privacy policy. Diagnostic logging and product analytics are different data categories and should not be conflated.

## 10. Acceptance checklist

- [ ] Structured logs and stable error codes exist.
- [ ] Rotation, size and retention limits are configured.
- [ ] Sensitive values are redacted.
- [ ] Crash/health strategy is documented.
- [ ] Diagnostic bundle is useful and privacy-reviewed.
- [ ] Support can identify exact product/build from diagnostics.
