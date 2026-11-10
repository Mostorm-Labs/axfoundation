# SF-09 — Security & Identity

- **Status:** Proposed
- **Version:** 1.0
- **Owner:** Product Security / Software Engineering
- **Related:** SF-03, SF-04, SF-05, SF-06, SF-07, SF-08

## 1. Purpose

Establish baseline security requirements for software build, distribution, runtime identity, device communication, licensing and support operations.

## 2. Threat model

Each product MUST identify:
- assets to protect;
- trust boundaries and privileged components;
- users, devices, services and external dependencies;
- attacker capabilities relevant to deployment;
- security-critical operations;
- recovery and incident response assumptions.

A desktop utility, cloud service, room appliance and firmware updater have different threat models. Do not copy a generic checklist without assessing actual exposure.

## 3. Identity

Distinguish:
- product identity;
- installation identity;
- device identity;
- user identity;
- service/workload identity.

A device identifier SHOULD be stable for the intended lifecycle but MUST NOT be treated as proof of authenticity by itself. Avoid relying solely on easily copied hardware serials or MAC addresses. Where device authenticity matters, use cryptographic credentials protected by TPM, secure element, OS keystore or an equivalent mechanism where available.

Define provisioning, rotation, revocation, factory reset, replacement and recovery behavior.

## 4. Authentication and authorization

- Authenticate remote peers and services.
- Use TLS with certificate validation for network APIs.
- Apply least privilege to processes, services, CI jobs and operators.
- Separate authentication (who/what) from authorization (allowed action).
- Enforce authorization at the resource/action boundary.
- Define credential expiry, rotation and revocation.
- Protect local IPC against unauthorized local processes.

## 5. Secrets and key management

Secrets MUST NOT be embedded in source code, public binaries, logs or ordinary configuration. Use a platform credential store or managed secret service. Signing keys and production credentials MUST be access-controlled, auditable and separated from normal build execution.

Define key generation, storage, rotation, backup, revocation and incident response. A key compromise must have a documented recovery path.

## 6. Software supply chain

Release pipelines SHOULD:
- pin dependencies and toolchains;
- scan dependencies and known vulnerabilities;
- record licenses;
- generate an SBOM for shipped products where practical;
- produce build provenance;
- sign production artifacts;
- protect release credentials;
- restrict workflow permissions.

Consumers MUST verify update authenticity before applying packages (SF-04/SF-05).

## 7. Secure defaults

- Disable debug endpoints and verbose sensitive logging in production.
- Bind local admin interfaces narrowly.
- Validate all external inputs.
- Set resource limits for network and IPC payloads.
- Use secure random generation for tokens and identifiers requiring unpredictability.
- Fail closed for authentication and signature verification.
- Do not expose secrets in crash or diagnostic bundles.

## 8. Licensing

License enforcement SHOULD be isolated from product business logic behind a stable API. Define online activation, offline activation, rehost, factory provisioning, recovery, clock/tamper assumptions and server unavailability behavior. Do not equate device identity with license entitlement.

## 9. Security incident handling

Products MUST define a reporting and response path for:
- compromised signing credentials;
- vulnerable dependencies;
- unauthorized device/service access;
- update-channel compromise;
- exposed customer secrets or personal data.

The process should cover triage, containment, key revocation, fixed release, customer communication and evidence preservation.

## 10. Acceptance checklist

- [ ] Threat model and trust boundaries exist.
- [ ] Device/user/service identities are distinguished.
- [ ] Secrets are stored and rotated safely.
- [ ] Production artifacts are authenticated.
- [ ] CI permissions and signing keys are protected.
- [ ] Diagnostics are privacy-reviewed.
- [ ] Security incident and key-compromise recovery are documented.
