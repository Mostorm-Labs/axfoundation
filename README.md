# AX Foundation

**Company-wide Software Foundation Standards**

AX Foundation defines the engineering standards, reusable contracts, tooling boundaries, and platform adapters shared by software products developed by Mostorm-Labs.

The goal is not to force every product into one language or framework. The goal is to make projects consistent in the areas that should be consistent: version identity, repository hygiene, build and release evidence, distribution, installation and update safety, diagnostics, configuration, runtime contracts, security, and compatibility.

## Status

- Specification set: **v1.0 — Initial baseline**
- Authority: **Proposed; pending engineering review and adoption**
- Scope: new software projects and materially changed existing projects
- Normative terms: **MUST**, **MUST NOT**, **SHOULD**, **SHOULD NOT**, and **MAY** are used in their usual standards meaning.

These documents are a starting baseline, not a claim that every platform implementation already exists. Platform-specific constraints and exceptions must be documented rather than hidden.

## Documents

| ID | Standard | Scope |
|---|---|---|
| SF-00 | [Foundation Charter](docs/SF-00-foundation-charter.md) | Purpose, principles, governance and adoption |
| SF-01 | [Software Versioning](docs/SF-01-software-versioning.md) | Product/build/schema/protocol versions and compatibility policy |
| SF-02 | [Repository Standard](docs/SF-02-repository-standard.md) | Repository layout, Git workflow, review and dependency hygiene |
| SF-03 | [Build & CI/CD](docs/SF-03-build-and-cicd.md) | Reproducible builds, pipeline stages, artifacts and release gates |
| SF-04 | [Release & Distribution](docs/SF-04-release-and-distribution.md) | Artifact identity, manifests, channels, storage and rollout |
| SF-05 | [Installation & Update](docs/SF-05-installation-and-update.md) | Install, download, update, recovery and rollback |
| SF-06 | [Logging & Diagnostics](docs/SF-06-logging-and-diagnostics.md) | Structured logs, crash data, health and diagnostic bundles |
| SF-07 | [Configuration Management](docs/SF-07-configuration-management.md) | Configuration sources, schema, precedence, secrets and migration |
| SF-08 | [Runtime Foundation](docs/SF-08-runtime-foundation.md) | Lifecycle, storage, network, IPC, tasks and platform adapters |
| SF-09 | [Security & Identity](docs/SF-09-security-and-identity.md) | Device identity, trust, authorization, secrets and software integrity |
| SF-10 | [Compatibility & Testing](docs/SF-10-compatibility-and-testing.md) | Compatibility matrix, test strategy, golden corpus and release evidence |

## Recommended reading order

1. SF-00 — understand scope and governance.
2. SF-01 and SF-10 — define compatibility promises before implementation.
3. SF-02 and SF-03 — establish the development and build workflow.
4. SF-04 and SF-05 — define how software is delivered and safely updated.
5. SF-06 through SF-09 — define operational and runtime contracts.

## Adoption model

Each product creates a short **Foundation Adoption Record** identifying:
- applicable standards and implementation status;
- platform, architecture, distribution model and deployment environment;
- selected tools and platform-specific adapters;
- deviations, rationale, risk owner and expiry/review date;
- evidence links for required checks.

A project may adopt standards incrementally. Deviations require explicit approval; silently ignoring a MUST requirement is not an accepted deviation.

## Repository structure

```
docs/
  SF-00-foundation-charter.md
  SF-01-software-versioning.md
  ...
  SF-10-compatibility-and-testing.md
```

Reusable code, CI templates, schemas and tools should be added only after the relevant contracts are reviewed and validated in at least one real product. This repository initially establishes the standards and boundaries; it is not yet a universal runtime SDK.
