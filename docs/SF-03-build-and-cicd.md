# SF-03 — Build & CI/CD

- **Status:** Proposed
- **Version:** 1.0
- **Owner:** Developer Infrastructure
- **Related:** SF-01, SF-02, SF-04, SF-10

## 1. Purpose

Make code validation, build, packaging and release evidence repeatable. CI/CD templates should standardize pipeline stages while allowing each project to supply its native toolchain.

## 2. Pipeline classes

| Pipeline | Trigger | Minimum responsibility |
|---|---|---|
| Pull Request | PR opened/updated | Format/lint, static checks, unit tests, compile |
| Integration | Merge to default branch | Full target build, integration tests, artifact smoke test |
| Nightly | Scheduled | Expanded regression, compatibility, long-running tests |
| Release | Approved tag or manual approval | Reproducible release build, signing, SBOM, provenance, publish |
| Hotfix | Approved hotfix branch/tag | Focused validation plus mandatory release security/integrity checks |

## 3. Standard stages

1. **Checkout:** resolve exact commit and submodules.
2. **Environment:** provision pinned compiler, SDK, toolchain and dependencies.
3. **Quality:** formatting, lint, static analysis and policy checks.
4. **Test:** unit, component, integration and compatibility tests appropriate to the change.
5. **Build:** compile declared OS/architecture/configuration matrix.
6. **Package:** create native distributables and metadata.
7. **Verify:** inspect package, run install/launch smoke tests, validate checksums.
8. **Secure:** scan dependencies, produce SBOM, sign artifacts and record provenance.
9. **Publish:** upload immutable artifacts and release metadata.
10. **Deploy:** promote through environments/channels with approval and health checks.

A project MAY omit inapplicable stages, but MUST record why.

## 4. Build reproducibility

Projects SHOULD pin:
- compiler and SDK versions;
- build tools and plugins;
- dependency versions and lockfiles;
- build flags and feature configuration;
- container images or runner images where used.

Builds MUST record source SHA and toolchain identity. Release artifacts MUST be built from a clean, known source revision. Avoid depending on untracked local files or developer machine state.

## 5. Build matrix

The project defines a supported matrix of:
- OS and minimum OS version;
- CPU architecture;
- compiler/toolchain;
- build configuration;
- hardware variant where relevant.

Do not build every theoretical combination. The matrix should cover supported products and high-risk compatibility boundaries.

## 6. Quality gates

At minimum, PR CI SHOULD run formatting/lint, compilation and unit tests. Release CI MUST run the tests designated as release-blocking by the product's verification plan.

Flaky tests MUST be tracked and corrected; silently disabling a failing test is not an acceptable permanent workaround. Exceptions require owner, rationale and expiry.

## 7. Artifact contract

Every produced artifact MUST be associated with:
- repository and source commit;
- product/build version;
- workflow run and build configuration;
- target platform and architecture;
- artifact filename, size and SHA-256 digest;
- signing status and signer identity where applicable;
- SBOM and provenance references where supported.

Artifacts MUST be immutable after publication. A corrected build receives a new build identity.

## 8. Secrets and permissions

Use least-privilege workflow permissions. Separate build credentials from release-signing credentials. Signing keys MUST NOT be exposed to untrusted pull-request workflows. Production publication SHOULD require protected environments or explicit approval.

## 9. Deployment safety

Deployments SHOULD support staged promotion (internal → beta → stable), health checks and stop/rollback controls. Deployment success must be based on observable outcomes, not merely a successful upload.

## 10. Tooling guidance

GitHub Actions is a suitable default for repositories hosted on GitHub. Self-hosted runners may be needed for Windows signing, specialized SDKs, hardware access or network-isolated builds. Runner images and maintenance ownership must be documented.

## 11. Acceptance checklist

- [ ] PR pipeline runs required checks.
- [ ] Release build uses a clean, identified revision.
- [ ] Target matrix is explicit.
- [ ] Artifact metadata and digest are generated.
- [ ] Secrets are isolated from untrusted workflows.
- [ ] Release artifacts are immutable and traceable.
- [ ] Failed release checks prevent publication unless an approved exception exists.
