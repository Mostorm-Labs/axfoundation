# SF-01 — Software Versioning

- **Status:** Proposed
- **Version:** 1.0
- **Owner:** Software Engineering
- **Related:** SF-04, SF-05, SF-07, SF-10

## 1. Purpose

Define stable, unambiguous version identities for products, builds, protocols, persisted data and firmware. A single application version MUST NOT be used as a substitute for every independently evolving contract.

## 2. Version dimensions

| Dimension | Example | Changes when |
|---|---|---|
| Product version | 2.4.1 | User-visible product release changes |
| Build identity | 20261110.1842+gabc1234 | A particular build is produced |
| Package revision | 2.4.1-3 | Packaging changes without product behavior change |
| Protocol version | 1.3 | Wire contract changes under its compatibility policy |
| Data/schema version | 5 | Persisted representation or migration contract changes |
| Firmware version | 1.8.0 | Device firmware release changes |
| Updater version | 1.2.0 | Update engine changes |
| SDK/API version | 3.1.0 | Public library contract changes |

These dimensions MUST be independently managed where they have independent compatibility lifecycles.

## 3. Product versioning

Use Semantic Versioning (MAJOR.MINOR.PATCH) for public product releases where practical:
- MAJOR: incompatible change to a published public contract.
- MINOR: backward-compatible capability addition.
- PATCH: backward-compatible fixes.

Pre-release identifiers MAY be used, e.g. `2.5.0-beta.2`. Build metadata MAY identify CI run or commit but MUST NOT be used to determine precedence.

If a platform ecosystem requires a different package version format, maintain an explicit mapping to the canonical product version.

## 4. Build identity

Every distributable build MUST expose:
- product identifier and product version;
- build number or immutable build ID;
- source commit SHA;
- build timestamp in UTC;
- target OS/platform and CPU architecture;
- build configuration (release/debug and relevant feature profile);
- package format and package revision;
- dependency lock/snapshot identity where available.

Build identity MUST be immutable. Rebuilding the same version with different contents MUST produce a distinguishable build identity and artifact digest.

## 5. Compatibility dimensions

Version comparison alone is insufficient. Projects MUST separately define:
- minimum and maximum supported OS/runtime versions;
- CPU architecture and hardware compatibility;
- client/server protocol compatibility;
- persisted data/schema compatibility;
- plugin/SDK/API compatibility;
- firmware/bootloader compatibility;
- updater/package compatibility.

Capability negotiation SHOULD be used for optional protocol features rather than relying only on client version strings.

## 6. Version compatibility policy

Each product MUST publish a compatibility policy that specifies:
- supported version window;
- minimum supported upgrade source;
- whether skipped-version upgrades are supported;
- data migration behavior;
- rollback constraints;
- end-of-support policy;
- behavior for unknown or unsupported versions.

New versions SHOULD read supported historical data. Unknown fields and extensions MUST be preserved when safe, or writes MUST be blocked to avoid destructive round trips. Unsupported semantics MUST NOT be silently reinterpreted.

## 7. Version sources of truth

The product repository MUST define one authoritative version source or a deterministic generation process. Package metadata, UI display, update manifests and diagnostics MUST derive from that source rather than manually maintained copies.

A release tag MUST point to the exact source revision used for release. Tags for published releases MUST NOT be moved or reused.

## 8. Release metadata

Release metadata SHOULD include product version, build identity, channel, target platform, architecture, minimum OS, protocol/schema compatibility declarations, artifact URL, size, digest, signature metadata, release notes and publication time.

See SF-04 for the manifest contract.

## 9. Deprecation and end of support

A public contract MUST NOT be removed without:
1. identifying affected consumers and data;
2. publishing a deprecation notice;
3. defining a migration or replacement;
4. defining the earliest removal release;
5. validating the supported upgrade path.

Security emergencies MAY require accelerated deprecation, with explicit impact and mitigation notes.

## 10. Acceptance checklist

- [ ] Product and build versions are distinct.
- [ ] Protocol, schema and firmware versions are independently represented where applicable.
- [ ] Version is generated from an authoritative source.
- [ ] Artifacts identify source commit and target platform.
- [ ] Compatibility windows and EOL policy are documented.
- [ ] Release tags are immutable.
