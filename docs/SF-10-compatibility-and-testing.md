# SF-10 — Compatibility & Testing

- **Status:** Proposed
- **Version:** 1.0
- **Owner:** Quality Engineering / Software Architecture
- **Related:** SF-01 through SF-09

## 1. Purpose

Make compatibility an explicit product contract and a repeatable release verification activity. Compatibility is multidimensional; a single application version comparison is insufficient.

## 2. Compatibility dimensions

Every product defines applicable dimensions:

| Dimension | Example question |
|---|---|
| OS/runtime | Does this release support the declared Windows/Linux versions? |
| CPU architecture | x64, ARM64, ARMv7, etc. |
| Hardware | Which device/board/revision variants are supported? |
| Protocol | Can this client communicate with the supported server/peer versions? |
| Persisted data | Can this version read and safely write historical data? |
| API/SDK | Are public interfaces source/binary compatible? |
| Firmware/bootloader | Is the firmware compatible with bootloader and peripherals? |
| Installer/updater | Can this package upgrade the installed version safely? |
| Configuration | Can old configuration be migrated without loss? |
| Plugin/extension | Are extension contracts and runtime dependencies compatible? |

## 3. Compatibility matrix

Maintain a versioned matrix with:
- product/client/server version;
- OS/runtime and architecture;
- hardware/firmware revision;
- protocol/schema version;
- supported operations;
- known limitations;
- support status and end-of-support date;
- test evidence link.

Use explicit states such as **supported**, **supported with limitations**, **unsupported**, and **not tested**. Do not present untested combinations as supported.

## 4. Compatibility principles

- New releases SHOULD read all historical data formats within the declared support policy.
- Unknown optional fields SHOULD be preserved across read/write cycles where feasible.
- If an older client cannot preserve unknown data, it MUST warn and prevent destructive save or use a documented safe mode.
- New protocol fields SHOULD be optional with safe defaults when possible.
- Breaking protocol changes require explicit negotiation, versioned endpoints or adapters.
- Capability negotiation is preferred for optional features.
- Schema migration MUST be deterministic, testable and observable.
- Binary rollback and data rollback are separate concerns.
- Compatibility guarantees have a defined support window; "forever" must not be assumed without an explicit archival strategy.

## 5. Test layers

| Test layer | What it proves |
|---|---|
| Unit | Local logic and boundary conditions |
| Contract | API/message/schema behavior |
| Integration | Components work together |
| System/E2E | Product workflows operate end to end |
| Compatibility | Declared version/platform combinations work |
| Migration | Historical data/config can be upgraded safely |
| Install/update | Install, upgrade, interruption and recovery work |
| Security | Threat-specific controls and abuse cases |
| Performance/stability | Resource, latency and long-running behavior meet targets |
| Hardware-in-loop | Real hardware/driver/device behavior works |

## 6. Golden corpus

For file formats, protocols, configuration and migrations, retain representative fixtures from released versions. The corpus SHOULD include:
- normal cases;
- boundary and maximum-size cases;
- optional/unknown fields;
- deprecated fields;
- malformed or truncated inputs;
- real-world anonymized samples where legally and operationally permitted.

Fixtures must identify their source version and expected behavior. Avoid including customer secrets or personal data.

## 7. Required release scenarios

Where applicable, test:
- current release reading historical supported data;
- older supported clients communicating with the new server;
- new client communicating with older supported server;
- unknown field preservation;
- unsupported feature downgrade/read-only behavior;
- upgrade from minimum supported source version;
- skipped-version upgrade;
- interrupted download/install;
- post-update health check and rollback;
- configuration/database migration and recovery;
- OS/architecture/hardware boundary combinations.

## 8. Test evidence and release decision

Each release MUST identify:
- tested product/build identity;
- target matrix and excluded combinations;
- test suite and result;
- known failures and approved exceptions;
- migration/rollback evidence;
- unresolved compatibility risks;
- release approver.

A passing build is not equivalent to a compatible release. A test result is meaningful only when its tested artifact and environment are identifiable.

## 9. Support lifecycle

Each product defines support windows for product versions, OS versions, hardware revisions, protocols and data schemas. End-of-support requires advance notice where feasible and a migration path. Security or platform constraints may shorten support with documented risk and communication.

## 10. Acceptance checklist

- [ ] Compatibility dimensions and support windows are declared.
- [ ] Matrix distinguishes supported from untested.
- [ ] Historical fixtures are retained.
- [ ] Upgrade, migration and rollback paths are tested.
- [ ] Unknown data behavior is safe.
- [ ] Release evidence identifies exact artifact and environment.
