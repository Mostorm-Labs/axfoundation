# SF-02 — Repository Standard

- **Status:** Proposed
- **Version:** 1.0
- **Owner:** Software Engineering
- **Related:** SF-01, SF-03, SF-10

## 1. Purpose

Provide a predictable repository structure and collaboration workflow without imposing a monorepo or a specific language.

## 2. Repository baseline

Every maintained product repository SHOULD contain:
- `README.md`: purpose, status, quick start and supported platforms;
- `LICENSE` or a clear proprietary notice;
- `CONTRIBUTING.md`: development and review workflow;
- `CHANGELOG.md` or release-note source;
- `docs/`: architecture, decisions, operations and compatibility;
- build and dependency lock files;
- automated tests;
- CI workflow definitions;
- security reporting instructions appropriate to repository visibility.

Generated outputs, credentials, local IDE state and machine-specific paths MUST NOT be committed unless explicitly required and documented.

## 3. Repository organization

Choose repository boundaries based on ownership, release cadence, dependency direction and independent deployment. Avoid splitting code into repositories solely for theoretical reuse. A shared component should have an explicit owner, API and release policy.

For multi-platform projects, keep common code and platform adapters visibly separated. Do not hide platform-specific behavior behind ambiguous conditional compilation spread throughout business logic.

## 4. Git workflow

Recommended default:
- protect the default branch;
- use short-lived feature/fix branches;
- require Pull Requests for shared branches;
- require CI checks before merge;
- require at least one qualified reviewer for production code;
- prohibit force-push to protected release branches;
- use signed tags or equivalent release provenance where supported.

Branch naming SHOULD be descriptive, e.g. `feature/<scope>`, `fix/<issue>`, `docs/<scope>`.

Commit messages SHOULD state intent and scope. Conventional Commits MAY be adopted if release automation benefits from it.

## 5. Pull Request requirements

A PR SHOULD include:
- problem and intended outcome;
- scope and non-goals;
- implementation summary;
- test evidence;
- compatibility/data migration impact;
- security/privacy impact;
- operational or rollout impact;
- screenshots or recordings for UI changes where useful;
- linked issue/decision.

PRs MUST NOT merge with known failing required checks unless an approved, time-limited exception is recorded.

## 6. Dependency hygiene

- Pin direct dependencies and commit lockfiles where supported.
- Record compiler, SDK and toolchain versions.
- Prefer reproducible dependency sources.
- Review license and security implications of new dependencies.
- Define ownership and update cadence for critical dependencies.
- Avoid floating production dependencies.
- Document vendored code and its upstream revision.

Dependency updates SHOULD be automated where practical, but automated PRs still require CI and review.

## 7. Secrets and configuration

Secrets MUST NOT be committed. Use GitHub Actions secrets, an approved secret manager or protected environment variables. Example configuration files MUST contain placeholders only.

## 8. Branch and release protection

Production repositories SHOULD enforce:
- required status checks;
- review requirements;
- restricted direct pushes;
- release tag policy;
- CODEOWNERS for critical areas where practical.

## 9. Repository readiness checklist

- [ ] README explains build, test and supported targets.
- [ ] Dependencies and toolchains are pinned or constrained.
- [ ] PR review and required CI are enabled.
- [ ] No secrets or generated artifacts are committed.
- [ ] Release and compatibility documentation exists.
- [ ] Critical ownership boundaries are visible.
