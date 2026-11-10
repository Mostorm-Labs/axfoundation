# SF-05 — Installation & Update

- **Status:** Proposed
- **Version:** 1.0
- **Owner:** Client Platform Engineering
- **Related:** SF-01, SF-04, SF-06, SF-07, SF-09, SF-10

## 1. Purpose

Define safe and observable installation, download, update, uninstall and recovery behavior across desktop, mobile, web and embedded deployment models.

## 2. Separate responsibilities

- **Distribution:** identifies available releases and provides artifacts (SF-04).
- **Download manager:** transfers bytes and reports transfer state.
- **Installer/package manager:** applies platform-native installation semantics.
- **Updater/orchestrator:** coordinates eligibility, staging, shutdown, install, health confirmation and recovery.
- **Application:** reports version and health; it should not overwrite its own active executable image.

These may be separate processes or platform services. Their ownership and trust boundaries must be explicit.

## 3. Update state machine

Recommended states:

`IDLE → CHECKING → AVAILABLE → DOWNLOADING → VERIFIED → STAGED → APPLYING → HEALTH_CHECK → SUCCEEDED`

Failure/recovery states include `CANCELLED`, `FAILED_RETRYABLE`, `FAILED_FINAL`, `ROLLBACK_REQUIRED` and `ROLLED_BACK`.

Each transition MUST have:
- an owning component;
- durable state where interruption is possible;
- timeout and retry policy;
- user-visible or diagnosable outcome;
- cleanup behavior.

## 4. Eligibility checks

Before download/install, validate:
- product and package identity;
- current and target versions;
- platform, architecture and hardware model;
- minimum OS/runtime/bootloader;
- free disk space and power requirements where relevant;
- package dependencies;
- channel and rollout eligibility;
- license or authorization if required;
- whether the update is blocked by an active operation.

Unsupported combinations MUST be rejected with a stable error code.

## 5. Download requirements

Where the platform permits, the download manager SHOULD support:
- HTTPS with certificate validation;
- resumable/range downloads;
- progress and throughput reporting;
- bounded retries with backoff and jitter;
- cancellation;
- timeout and stalled-transfer detection;
- mirror fallback;
- proxy support;
- temporary-file staging;
- digest and signature verification.

A partial download MUST NOT be treated as an installable artifact.

## 6. Apply and recovery

- Verify authenticity and integrity before execution.
- Use platform-native installation mechanisms where possible.
- Do not replace an executing binary in place.
- Preserve user data and configuration separately from replaceable application files.
- Record update transaction state before destructive steps.
- Define behavior after power loss, process crash or device reboot.
- Run post-install health checks before marking success.
- Keep a known-good recovery path when the platform supports it.

Desktop applications may use an external updater/launcher. Linux services may coordinate with package managers or systemd. Embedded devices SHOULD consider signed A/B images, boot-attempt limits and automatic fallback. Mobile apps MUST follow app-store/MDM and OS restrictions. Web applications use deployment/versioning and cache invalidation strategies rather than native installers.

## 7. Database and user-data migration

Application update and user-data migration are separate operations. Migrations MUST be versioned, ordered, observable and tested against representative historical data. Destructive migrations require backup/recovery strategy. Rollback after a schema migration may require a forward-fix rather than binary downgrade.

Never assume that reinstalling an older application safely reverses a newer data migration.

## 8. Update policy

Each product MUST define:
- manual/automatic update behavior;
- user consent and maintenance windows;
- mandatory update conditions;
- skipped-version support;
- offline update path where required;
- channel selection and downgrade policy;
- update retry/backoff;
- recovery and support procedure.

Security-critical forced updates require a documented policy and clear user/device impact.

## 9. Security

Updater components are privileged security-sensitive software. They MUST authenticate update metadata, verify signatures, prevent downgrade attacks where required, protect temporary files and avoid executing untrusted paths. Signing keys MUST remain outside client devices.

## 10. Acceptance checklist

- [ ] Update state machine and owner are documented.
- [ ] Eligibility and compatibility checks exist.
- [ ] Integrity and signature are verified before apply.
- [ ] Interrupted download/apply behavior is tested.
- [ ] Health confirmation is required before success.
- [ ] Recovery/rollback limits are explicit.
- [ ] Data migration and binary rollback are treated separately.
