# SF-07 — Configuration Management

- **Status:** Proposed
- **Version:** 1.0
- **Owner:** Software Architecture
- **Related:** SF-01, SF-06, SF-08, SF-09, SF-10

## 1. Purpose

Provide predictable configuration behavior across environments and versions, while separating configuration from application binaries and protecting secrets.

## 2. Configuration categories

Distinguish:
- built-in defaults;
- deployment/package defaults;
- machine/device configuration;
- user preferences;
- organization/administrator policy;
- remotely managed configuration;
- secrets and credentials.

Do not treat all configuration as one flat file with unclear ownership.

## 3. Precedence

Each product MUST define precedence. A recommended default, from lowest to highest, is:

`built-in defaults < deployment defaults < machine/device config < user preferences < administrator policy`

Remote policy precedence must be explicitly defined; it should not silently override a higher-trust local safety constraint. Secrets are managed separately and are not ordinary configuration values.

## 4. Schema

Configuration MUST have a schema or equivalent validation contract specifying:
- key and type;
- default;
- allowed range/enum;
- required/optional status;
- owner and scope;
- sensitivity classification;
- introduced/deprecated version;
- migration behavior.

Invalid configuration MUST produce a diagnosable error and a safe fallback or explicit refusal. Do not silently coerce values in ways that change behavior unexpectedly.

## 5. Persistence and migration

Configuration format MUST be versioned independently of product version. Migrations SHOULD be deterministic, idempotent and testable. Preserve unknown keys when safe. If a configuration cannot be safely interpreted, retain the original and report the problem rather than overwriting it.

User data, operational state and configuration SHOULD use separate storage locations and backup policies.

## 6. Secrets

Passwords, API tokens, private keys and long-lived credentials MUST NOT be stored as plaintext in ordinary config files. Use platform credential stores or an approved secret-management service. Secret values MUST be redacted from logs, crash reports and diagnostic bundles.

## 7. Remote configuration and feature flags

Remote configuration SHOULD be used for controlled rollout, environment-specific settings and feature flags—not as a substitute for a stable local contract.

Requirements:
- authenticated transport and server identity validation;
- schema/version validation;
- explicit scope (product, tenant, device cohort);
- expiry/TTL and stale-config behavior;
- audit trail for changes;
- rollback to last-known-good configuration;
- safe defaults when offline.

Remote flags MUST NOT be the only protection for security-critical behavior. Local authorization and safety checks remain authoritative.

## 8. Change behavior

Define whether changes apply immediately, on component restart or on application restart. Configuration updates SHOULD be atomic. Concurrent writers require locking or a single-writer ownership rule.

## 9. Acceptance checklist

- [ ] Configuration categories and precedence are documented.
- [ ] Schema validation and defaults exist.
- [ ] Config version and migration strategy exist.
- [ ] Secrets use secure storage.
- [ ] Remote config has authentication, expiry and rollback behavior.
- [ ] Logs and support bundles redact sensitive values.
