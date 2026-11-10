# SF-04 — Release & Distribution

- **Status:** Proposed
- **Version:** 1.0
- **Owner:** Release Engineering
- **Related:** SF-01, SF-03, SF-05, SF-09

## 1. Purpose

Define how signed, identifiable software artifacts move from CI to users, devices and customer environments.

## 2. Artifact identity

An artifact is identified by product ID, product version, build ID, target platform, architecture, package format and cryptographic digest. Filenames are descriptive but MUST NOT be treated as identity or integrity proof.

Published artifacts MUST be immutable. Replacing bytes at an existing release URL is prohibited; publish a new build/revision.

## 3. Release channels

Recommended channel semantics:
- **dev:** continuously changing engineering builds; no customer stability promise.
- **beta:** pre-release validation with explicit feedback expectations.
- **stable:** supported production release.
- **lts:** optional long-term support line with separate maintenance commitment.
- **customer/private:** controlled distribution for named customers or environments.

A product may use fewer channels. Channel promotion MUST be explicit and auditable.

## 4. Release manifest

A machine-readable manifest SHOULD contain at least:

```json
{
  "schema_version": 1,
  "product_id": "com.example.product",
  "release": {
    "version": "2.4.1",
    "build_id": "20261110.1842",
    "channel": "stable",
    "published_at": "2026-11-10T10:00:00Z"
  },
  "target": {
    "platform": "windows",
    "architecture": "x64",
    "min_os_version": "10.0.19045"
  },
  "artifact": {
    "filename": "product-2.4.1-win-x64.msi",
    "url": "https://downloads.example.com/...",
    "size": 12345678,
    "sha256": "<hex-digest>",
    "signature": {
      "format": "authenticode",
      "key_id": "<signing-key-id>"
    }
  },
  "compatibility": {
    "protocol": { "min": "1.0", "max": "1.x" },
    "data_schema": { "read": ["1", "2", "3"], "write": "3" }
  },
  "rollout": {
    "percentage": 100,
    "mandatory_after": null
  },
  "release_notes_url": "https://..."
}
```

This is illustrative, not a finalized JSON Schema. A production schema MUST define required fields, validation, signature envelope, URL policy, rollback semantics and forward-compatibility behavior. Manifest schema version is independent of product version.

## 5. Distribution service responsibilities

A distribution service or storage/CDN arrangement SHOULD provide:
- immutable artifact storage;
- manifest lookup by product/channel/platform/architecture;
- authentication and authorization for private releases;
- resumable downloads and range requests where supported;
- regional mirrors/CDN;
- download observability without collecting unnecessary personal data;
- retention and archival policy.

The client MUST validate the downloaded artifact independently of transport success.

## 6. Integrity and authenticity

SHA-256 detects corruption but does not prove publisher identity. Production artifacts MUST use a trusted digital signature or equivalent authenticated release metadata. Clients MUST verify signature and digest before installation. Trust roots and key rotation/revocation procedures MUST be documented.

## 7. Rollout

Stable releases SHOULD support staged rollout where the deployment model permits:
1. internal validation;
2. limited beta/customer cohort;
3. small stable cohort;
4. wider rollout;
5. full availability.

Rollout controls MUST be reversible. A bad release can be withdrawn or superseded, but already installed software requires an explicit recovery/update plan.

## 8. Retention and support

Define retention for:
- current stable and supported previous releases;
- installer and offline update packages;
- manifests and signatures;
- SBOM/provenance;
- release notes and test evidence.

Do not delete an artifact still required by a supported upgrade or recovery path.

## 9. Acceptance checklist

- [ ] Every artifact has immutable identity and digest.
- [ ] Manifest is validated against a versioned schema.
- [ ] Signature verification is defined and enforced.
- [ ] Channels and promotion rules are documented.
- [ ] Rollout can be paused or withdrawn.
- [ ] Supported recovery artifacts remain available.
