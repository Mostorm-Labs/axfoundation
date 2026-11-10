# SF-00 — Software Foundation Charter

- **Status:** Proposed
- **Version:** 1.0
- **Owner:** Software Engineering / Innovation Division
- **Applies to:** Software products, services, desktop/mobile applications, device agents, embedded software and their delivery pipelines

## 1. Purpose

AX Foundation reduces repeated engineering work and inconsistent product quality by defining shared software engineering contracts. It establishes what should be common across products while preserving legitimate differences between operating systems, languages, runtimes and deployment models.

## 2. Goals

1. New projects start from known engineering defaults rather than rediscovering basic practices.
2. Every shipped artifact has traceable source, version, build and release identity.
3. Software can be installed, updated, diagnosed and supported predictably.
4. Historical data and supported clients are protected by explicit compatibility policy.
5. Platform-specific behavior is isolated behind documented adapters.
6. Shared tooling is introduced only when a stable contract and demonstrated reuse exist.

## 3. Non-goals

- Mandating one programming language, UI framework, build system or package format.
- Replacing OS-native lifecycle, security, installer or application-store mechanisms.
- Building a distributed management platform before product needs justify it.
- Promising compatibility with every historical OS, device or client version indefinitely.
- Abstracting hardware-specific behavior into a lowest-common-denominator API.

## 4. Design principles

### 4.1 Contracts before shared code
Define semantics, inputs, outputs, failure behavior and compatibility guarantees before implementing a reusable library.

### 4.2 Common semantics, native realization
A shared interface may have different implementations on Windows, Linux, macOS, Android, iOS, Web and embedded targets. Platform limitations must remain visible.

### 4.3 Traceability by default
A release must be traceable to source revision, build workflow, dependencies, target platform, artifact digest and release channel.

### 4.4 Fail safely
Integrity failures, unsupported migrations, invalid configuration and incomplete updates must fail explicitly. Never silently discard data or bypass signature verification.

### 4.5 Least privilege and privacy
Collect only operational data needed to diagnose or operate the product. Protect credentials and redact sensitive values from logs and support bundles.

### 4.6 Incremental adoption
Prefer standards and templates first, then small libraries and tools, then centralized services where justified.

## 5. Capability taxonomy

| Domain | Shared contract | Platform-specific realization |
|---|---|---|
| Versioning | Version fields and compatibility policy | Native package/build metadata |
| Repository | Branch, review and dependency policy | Git hosting and repository settings |
| CI/CD | Pipeline stages and evidence | Runner, compiler and build system |
| Release | Artifact manifest and channel semantics | Storage, CDN and signing service |
| Install/update | State machine and safety invariants | MSI, DEB, app store, OTA, etc. |
| Diagnostics | Log schema, error taxonomy, bundle layout | OS log and crash APIs |
| Configuration | Schema, precedence and migration rules | Registry, files, preferences, remote config |
| Runtime | Lifecycle and service contracts | systemd, Windows Service, app lifecycle |
| Security | Identity and trust requirements | DPAPI, Keychain, Keystore, TPM, secure element |
| Compatibility | Matrix and evidence model | Platform-specific test environments |

## 6. Normative language

- **MUST / MUST NOT:** mandatory for conforming projects unless an approved deviation exists.
- **SHOULD / SHOULD NOT:** recommended; deviations require documented reasoning.
- **MAY:** optional.

A standard is not automatically implemented merely because it is documented. Each project records adoption status and evidence.

## 7. Governance and change control

- This repository is the canonical home for AX Foundation standards.
- Each standard has an ID, version, status, owner, scope and change history.
- Changes are proposed through Pull Requests.
- Breaking changes require a migration note and an explicit effective version.
- Product-specific exceptions belong in that product's adoption record, not as undocumented edits to the common standard.
- Standards are reviewed at least annually and when a significant security, platform or operational requirement changes.

## 8. Adoption record template

Each product should maintain `docs/foundation-adoption.md` with:

| Field | Required information |
|---|---|
| Product / owner | Product name and accountable engineering owner |
| Release model | Desktop, service, mobile, web, firmware, appliance |
| Platforms | OS versions, CPU architectures and hardware variants |
| Standards | SF-01…SF-10 status: adopted / partial / not applicable |
| Toolchain | Repository, CI, build, package and distribution tools |
| Update model | Manual, in-app, service-managed, store, OTA, offline |
| Diagnostics | Log location, retention, crash and support-bundle process |
| Deviations | Requirement, rationale, risk, approver and review date |
| Evidence | Links to tests, workflows, manifests and release records |

## 9. Conformance levels

- **Level 0 — Recorded:** project has an adoption record and declared exceptions.
- **Level 1 — Engineering baseline:** repository, versioning, CI, artifact traceability and basic diagnostics conform.
- **Level 2 — Operable release:** distribution, installation/update safety, compatibility tests and security controls conform.
- **Level 3 — Managed product:** remote diagnostics, staged rollout, health reporting and lifecycle governance are implemented where applicable.

A project should not claim a level without evidence.
