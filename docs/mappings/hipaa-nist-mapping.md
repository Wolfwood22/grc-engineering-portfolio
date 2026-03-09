# HIPAA Technical Safeguards (45 CFR §164.312) → NIST 800-53 Rev 5 Master Mapping

**Author:** GRC Engineering Portfolio
**Date:** 2026-03-09
**Version:** 1.0
**Scope:** 45 CFR §164.312 — Technical Safeguards only (excludes Administrative §164.308 and Physical §164.310)

> **NEOGOV Context:** NEOGOV is a SaaS platform serving state and local government HR departments. When those governments offer health benefits or handle medical leave, NEOGOV qualifies as a Business Associate under HIPAA. Business Associates must comply with the full HIPAA Security Rule, including all Technical Safeguard standards and "Required" implementation specifications. "Addressable" specs must either be implemented or formally documented as inapplicable with an equivalent alternative.

---

## HIPAA Technical Safeguards Structure

45 CFR §164.312 defines five standards. Each standard has a type (Required = must implement; Addressable = implement or document equivalent). Implementation specifications within a standard can independently be Required or Addressable.

| Standard | CFR Citation | Type | Implementation Specifications |
|---|---|---|---|
| Access Control | §164.312(a)(1) | Required | 4 sub-specs (2 Required, 2 Addressable) |
| Audit Controls | §164.312(b) | Required | None (standard itself is the requirement) |
| Integrity | §164.312(c)(1) | Required | 1 Addressable sub-spec |
| Person or Entity Authentication | §164.312(d) | Required | None |
| Transmission Security | §164.312(e)(1) | Required | 2 Addressable sub-specs |

**Total required implementation points:** 9 (5 standards + 4 specs), of which 3 are Addressable.

---

## Dual-Framework Priority Key

Controls marked **★ DUAL** appear in both this HIPAA mapping and the ISO 27001:2022 Annex A → NIST 800-53 Rev 5 mapping. These are the **highest-priority controls** for a commercial SaaS company satisfying both frameworks simultaneously. A single control implementation satisfies requirements in two audit domains.

Controls marked **HIPAA-ONLY** satisfy HIPAA requirements but have no direct ISO 27001 Annex A equivalent in the cross-reference mapping. These still require implementation for HIPAA compliance but do not contribute to ISO 27001 audit evidence.

---

## Standard 1 — Access Control (§164.312(a)(1)) — REQUIRED

> **Plain language:** Implement technical policies and procedures that allow only authorized persons or software programs to access ePHI.

### Standard-Level NIST Mapping

| NIST 800-53 Rev 5 Control | Control Title | Priority | ISO 27001 Cross-Ref |
|---|---|---|---|
| **AC-2** | Account Management | **★ DUAL** | A.8.2, A.5.15 |
| **AC-3** | Access Enforcement | **★ DUAL** | A.8.3 |
| **AC-6** | Least Privilege | **★ DUAL** | A.8.2 |
| **AC-17** | Remote Access | **★ DUAL** | A.8.3, A.6.7 |

---

### §164.312(a)(2)(i) — Unique User Identification — REQUIRED

> Assign a unique name and/or number for identifying and tracking user identity.

| NIST 800-53 Rev 5 Control | Control Title | Priority | ISO 27001 Cross-Ref | Implementation Notes |
|---|---|---|---|---|
| **IA-2** | Identification and Authentication (Organizational Users) | **★ DUAL** | A.5.16, A.8.5 | Prohibits shared accounts; each user must have a unique identifier |
| **IA-4** | Identifier Management | **★ DUAL** | A.5.16 | Governs lifecycle: assignment, reuse prevention, expiration of identifiers |
| **AC-2** | Account Management | **★ DUAL** | A.8.2, A.5.15 | Enforces 1:1 account-to-person mapping; shared/service accounts require documented exception |

**NEOGOV relevance:** Shared admin accounts for HR system configuration are a recurring audit finding under this spec. Every privileged action touching ePHI must be traceable to an individual.

---

### §164.312(a)(2)(ii) — Emergency Access Procedure — REQUIRED

> Establish and implement as needed procedures for obtaining necessary ePHI during an emergency.

| NIST 800-53 Rev 5 Control | Control Title | Priority | ISO 27001 Cross-Ref | Implementation Notes |
|---|---|---|---|---|
| **CP-2** | Contingency Plan | **★ DUAL** | A.5.29, A.5.30, A.8.6 | Documents emergency access procedures and triggers |
| **IR-4** | Incident Handling | **★ DUAL** | A.5.25, A.5.26 | Covers handling of security events that trigger emergency access |
| **AC-14** | Permitted Actions Without Identification or Authentication | HIPAA-ONLY | — | "Break-glass" access authorization with post-event audit review |
| **AC-2** | Account Management | **★ DUAL** | A.8.2, A.5.15 | Emergency/temporary account provisioning and automatic expiration |

**NEOGOV relevance:** For HR SaaS platforms, emergency access scenarios include ransomware response or cloud provider outage requiring direct database access to retrieve benefits enrollment records. The procedure must be documented, tested, and auditable.

---

### §164.312(a)(2)(iii) — Automatic Logoff — ADDRESSABLE

> Implement electronic procedures that terminate an electronic session after a predetermined time of inactivity.

| NIST 800-53 Rev 5 Control | Control Title | Priority | ISO 27001 Cross-Ref | Implementation Notes |
|---|---|---|---|---|
| **AC-11** | Device Lock | **★ DUAL** | A.7.7 | Session lock after inactivity; pattern-hiding display |
| **AC-12** | Session Termination | HIPAA-ONLY | — | Terminates sessions after conditions (inactivity timeout, logout, time-based) |
| **SC-23** | Session Authenticity | HIPAA-ONLY | — | Prevents session hijacking that would bypass logoff controls |

**Addressable note:** If automatic logoff is not implemented, the covered entity must document the reason and implement an equivalent alternative. Most SaaS web apps implement this via session token expiry — document the timeout value and rationale.

**NEOGOV relevance:** HR portal sessions handling benefits data should have a ≤15-minute idle timeout for government users working in shared workstation environments (common in agency settings).

---

### §164.312(a)(2)(iv) — Encryption and Decryption — ADDRESSABLE

> Implement a mechanism to encrypt and decrypt ePHI at rest.

| NIST 800-53 Rev 5 Control | Control Title | Priority | ISO 27001 Cross-Ref | Implementation Notes |
|---|---|---|---|---|
| **SC-28** | Protection of Information at Rest | **★ DUAL** | A.8.1, A.8.11, A.8.24 | Encryption of stored ePHI; covers databases, backups, snapshots |
| **SC-13** | Cryptographic Protection | **★ DUAL** | A.8.24 | Specifies approved cryptographic mechanisms (FIPS 140-2/3 for FedRAMP overlap) |
| **MP-5** | Media Transport | **★ DUAL** | A.7.9 | Encryption of physical media containing ePHI |

**Addressable note:** HHS guidance treats encryption as the de facto standard — an unencrypted breach triggers mandatory notification. In practice, "addressable" means implement it. The only valid alternative is a compensating control with documented justification explaining why encryption is unreasonable.

**NEOGOV relevance:** AWS RDS encryption at rest (AES-256) satisfies SC-28 for the database tier. Application-level field encryption for SSNs and medical identifiers satisfies SC-28 at higher assurance. Backup encryption must be separately verified — snapshot encryption is not automatic if volumes were created before encryption was enabled.

---

## Standard 2 — Audit Controls (§164.312(b)) — REQUIRED

> **Plain language:** Implement hardware, software, and/or procedural mechanisms that record and examine activity in information systems that contain or use ePHI.

*Note: §164.312(b) has no implementation specifications. The standard itself is Required.*

| NIST 800-53 Rev 5 Control | Control Title | Priority | ISO 27001 Cross-Ref | Implementation Notes |
|---|---|---|---|---|
| **AU-2** | Event Logging | **★ DUAL** | A.8.15 | Defines loggable events for ePHI-touching systems; login/logout, access, modification, deletion |
| **AU-3** | Content of Audit Records | **★ DUAL** | A.8.15 | Specifies required fields: timestamp, user ID, event type, outcome, source |
| **AU-6** | Audit Record Review, Analysis, and Reporting | **★ DUAL** | A.8.16 | Periodic review of logs; automated alerting on anomalous ePHI access |
| **AU-9** | Protection of Audit Information | **★ DUAL** | A.8.15, A.5.28 | Prevents tampering with audit logs; immutable log storage (WORM or cloud append-only) |
| **AU-12** | Audit Record Generation | **★ DUAL** | A.8.15 | Ensures every system component generates logs meeting AU-3 requirements |
| **AU-10** | Non-Repudiation | HIPAA-ONLY | — | Cryptographic evidence linking actions to identities; relevant for access audit disputes |

**NEOGOV relevance:** HIPAA audit controls require logging *all* access to ePHI, not just failed attempts. This means application-layer logging of every API call that retrieves benefits or health information, not just infrastructure-level authentication logs. CloudTrail alone is insufficient — application-level audit events (which records were accessed, by whom, when) are required.

---

## Standard 3 — Integrity (§164.312(c)(1)) — REQUIRED

> **Plain language:** Implement policies and procedures to protect ePHI from improper alteration or destruction.

### Standard-Level NIST Mapping

| NIST 800-53 Rev 5 Control | Control Title | Priority | ISO 27001 Cross-Ref | Implementation Notes |
|---|---|---|---|---|
| **SI-7** | Software, Firmware, and Information Integrity | HIPAA-ONLY | — | Integrity verification mechanisms; detects unauthorized modification of stored ePHI |
| **SC-8** | Transmission Confidentiality and Integrity | **★ DUAL** | A.8.24 | Protects ePHI integrity in transit |
| **AU-9** | Protection of Audit Information | **★ DUAL** | A.8.15, A.5.28 | Protects integrity of the audit trail itself |

---

### §164.312(c)(2) — Mechanism to Authenticate Electronic PHI — ADDRESSABLE

> Implement electronic mechanisms to corroborate that ePHI has not been altered or destroyed in an unauthorized manner.

| NIST 800-53 Rev 5 Control | Control Title | Priority | ISO 27001 Cross-Ref | Implementation Notes |
|---|---|---|---|---|
| **SI-7** | Software, Firmware, and Information Integrity | HIPAA-ONLY | — | Hash-based integrity checking; cryptographic checksums on ePHI records |
| **SC-8(1)** | Transmission Confidentiality and Integrity — Cryptographic Protection | **★ DUAL** | A.8.24 | TLS with integrity guarantees (AEAD cipher suites); prevents in-transit modification |
| **AU-10** | Non-Repudiation | HIPAA-ONLY | — | Provides cryptographic evidence that ePHI content was not altered post-transmission |
| **SI-10** | Information Input Validation | HIPAA-ONLY | — | Validates that ePHI inputs conform to expected formats, preventing injection-based corruption |

**Addressable note:** If a mechanism to authenticate ePHI is not implemented, document the alternative. Most SaaS databases with referential integrity constraints and application-layer validation satisfy the intent. A formal hash/checksum mechanism is best practice for audit defensibility.

**NEOGOV relevance:** Benefits enrollment records are authoritative — if an employee's coverage elections are silently corrupted, the impact is financial and legal. SHA-256 hash validation of exported enrollment data files and database checksum verification at the row level both satisfy this spec.

---

## Standard 4 — Person or Entity Authentication (§164.312(d)) — REQUIRED

> **Plain language:** Implement procedures to verify that a person or entity seeking access to ePHI is the one claimed.

*Note: §164.312(d) has no implementation specifications. The standard itself is Required.*

| NIST 800-53 Rev 5 Control | Control Title | Priority | ISO 27001 Cross-Ref | Implementation Notes |
|---|---|---|---|---|
| **IA-2** | Identification and Authentication (Organizational Users) | **★ DUAL** | A.5.16, A.8.5 | Core authentication requirement; username + password minimum |
| **IA-2(1)** | MFA for Privileged Accounts | **★ DUAL** | A.8.5 | MFA required for all privileged access to ePHI systems |
| **IA-2(6)** | Access to Accounts — Separate Device (Phishing-Resistant MFA) | **★ DUAL** | A.8.5 | Hardware token or device-bound credential for highest-assurance access |
| **IA-3** | Device Identification and Authentication | HIPAA-ONLY | — | Authenticates devices (not just users) accessing ePHI; relevant for API integrations and IoT/kiosk terminals |
| **IA-5** | Authenticator Management | **★ DUAL** | A.5.17, A.8.5 | Password complexity, rotation policy, compromised credential handling |
| **IA-8** | Identification and Authentication (Non-Organizational Users) | **★ DUAL** | A.8.5 | Authentication for external parties (employees, third-party admins) accessing ePHI |

**NEOGOV relevance:** Government HR users accessing benefits data through NEOGOV's portal must be authenticated. When agencies provide SSO via SAML/OIDC, NEOGOV inherits the agency's identity assurance level — the BAA should document what identity assurance the agency is contractually required to provide. For NEOGOV-managed credentials (service accounts, API keys), IA-5 is fully within NEOGOV's scope to implement.

---

## Standard 5 — Transmission Security (§164.312(e)(1)) — REQUIRED

> **Plain language:** Implement technical security measures to guard against unauthorized access to ePHI that is being transmitted over an electronic communications network.

### Standard-Level NIST Mapping

| NIST 800-53 Rev 5 Control | Control Title | Priority | ISO 27001 Cross-Ref | Implementation Notes |
|---|---|---|---|---|
| **SC-8** | Transmission Confidentiality and Integrity | **★ DUAL** | A.8.24 | Protects ePHI in transit against eavesdropping and modification |
| **SC-13** | Cryptographic Protection | **★ DUAL** | A.8.24 | Defines approved algorithms for transmission encryption |

---

### §164.312(e)(2)(i) — Integrity Controls — ADDRESSABLE

> Implement security measures to ensure that electronically transmitted ePHI is not improperly modified without detection until disposed of.

| NIST 800-53 Rev 5 Control | Control Title | Priority | ISO 27001 Cross-Ref | Implementation Notes |
|---|---|---|---|---|
| **SC-8** | Transmission Confidentiality and Integrity | **★ DUAL** | A.8.24 | TLS ensures integrity in transit via HMAC/AEAD |
| **SC-8(1)** | Cryptographic or Alternate Physical Protection | **★ DUAL** | A.8.24 | Mandates cryptographic protection specifically (TLS 1.2+ minimum; TLS 1.3 preferred) |
| **SI-7** | Software, Firmware, and Information Integrity | HIPAA-ONLY | — | End-to-end integrity verification beyond transport layer |

---

### §164.312(e)(2)(ii) — Encryption — ADDRESSABLE

> Implement a mechanism to encrypt ePHI whenever deemed appropriate.

| NIST 800-53 Rev 5 Control | Control Title | Priority | ISO 27001 Cross-Ref | Implementation Notes |
|---|---|---|---|---|
| **SC-8(1)** | Cryptographic or Alternate Physical Protection | **★ DUAL** | A.8.24 | TLS 1.2+ for all ePHI in transit; TLS 1.3 preferred |
| **SC-13** | Cryptographic Protection | **★ DUAL** | A.8.24 | Algorithm specification: AES-256, RSA-2048+, ECDHE for key exchange |
| **SC-28** | Protection of Information at Rest | **★ DUAL** | A.8.1, A.8.11, A.8.24 | Overlaps with §164.312(a)(2)(iv) — encryption is needed at-rest and in-transit |

**Addressable note:** Like §164.312(a)(2)(iv), HHS guidance makes this de facto Required. Any SaaS platform transmitting ePHI over public networks without TLS encryption has an indefensible audit position. All internal service-to-service calls (microservices, API gateways) also require encryption if they cross trust boundaries.

**NEOGOV relevance:** All NEOGOV API endpoints that return HR or benefits data must enforce TLS 1.2+ (not just permit it — enforce it by rejecting older protocol versions at the load balancer). Internal calls between NEOGOV's own services should also be encrypted if they transit outside a dedicated VPC subnet. Certificate management and rotation policy must be documented.

---

## Dual-Framework Priority Summary

Controls marked ★ DUAL satisfy requirements in **both** HIPAA §164.312 and ISO 27001:2022 Annex A. For a SaaS company pursuing both frameworks, every engineering hour spent on these controls generates cross-framework compliance value.

### Tier 1 — Highest-Priority Dual Controls (Authentication + Cryptography)

These controls appear in HIPAA's most-enforced areas (authentication and encryption) and in ISO 27001 Annex A.8 (Technological Controls).

| NIST Control | Control Title | HIPAA Spec | ISO 27001 Annex A |
|---|---|---|---|
| **IA-2** | Identification and Authentication | §164.312(d) Person Auth | A.5.16, A.8.5 |
| **IA-2(1)** | MFA — Privileged Accounts | §164.312(d) Person Auth | A.8.5 |
| **IA-2(6)** | Phishing-Resistant MFA | §164.312(d) Person Auth | A.8.5 |
| **IA-5** | Authenticator Management | §164.312(d) Person Auth | A.5.17, A.8.5 |
| **SC-8** | Transmission Confidentiality and Integrity | §164.312(e) Transmission Security | A.8.24 |
| **SC-8(1)** | Cryptographic Protection — TLS | §164.312(e)(2)(i)(ii) Encryption | A.8.24 |
| **SC-13** | Cryptographic Protection | §164.312(a)(2)(iv), §164.312(e)(2)(ii) | A.8.24 |
| **SC-28** | Protection of Information at Rest | §164.312(a)(2)(iv) Encryption at Rest | A.8.1, A.8.11, A.8.24 |

### Tier 2 — High-Priority Dual Controls (Access + Audit)

These controls satisfy HIPAA Access Control and Audit Controls standards and appear in ISO 27001 A.8 and A.5.

| NIST Control | Control Title | HIPAA Spec | ISO 27001 Annex A |
|---|---|---|---|
| **AC-2** | Account Management | §164.312(a)(1) Access Control | A.8.2, A.5.15 |
| **AC-3** | Access Enforcement | §164.312(a)(1) Access Control | A.8.3 |
| **AC-6** | Least Privilege | §164.312(a)(1) Access Control | A.8.2 |
| **IA-4** | Identifier Management | §164.312(a)(2)(i) Unique User ID | A.5.16 |
| **IA-8** | Auth — Non-Organizational Users | §164.312(d) Person Auth | A.8.5 |
| **AU-2** | Event Logging | §164.312(b) Audit Controls | A.8.15 |
| **AU-3** | Content of Audit Records | §164.312(b) Audit Controls | A.8.15 |
| **AU-6** | Audit Record Review | §164.312(b) Audit Controls | A.8.16 |
| **AU-9** | Protection of Audit Information | §164.312(b), §164.312(c) | A.8.15, A.5.28 |
| **AU-12** | Audit Record Generation | §164.312(b) Audit Controls | A.8.15 |

### Tier 3 — Supporting Dual Controls (Contingency + Physical)

| NIST Control | Control Title | HIPAA Spec | ISO 27001 Annex A |
|---|---|---|---|
| **AC-11** | Device Lock / Session Lock | §164.312(a)(2)(iii) Auto Logoff | A.7.7 |
| **AC-17** | Remote Access | §164.312(a)(1) Access Control | A.8.3, A.6.7 |
| **CP-2** | Contingency Plan | §164.312(a)(2)(ii) Emergency Access | A.5.29, A.5.30 |
| **IR-4** | Incident Handling | §164.312(a)(2)(ii) Emergency Access | A.5.25, A.5.26 |
| **MP-5** | Media Transport | §164.312(a)(2)(iv) Encryption | A.7.9 |

---

## HIPAA-Only Controls (No Direct ISO 27001 Equivalent)

These controls are required by HIPAA §164.312 but are not mapped to ISO 27001 Annex A controls in the cross-reference. They must be implemented for HIPAA compliance but do not reduce the ISO 27001 audit burden.

| NIST Control | Control Title | HIPAA Spec | Implementation Gap |
|---|---|---|---|
| **AC-12** | Session Termination | §164.312(a)(2)(iii) Auto Logoff | Explicit session termination logic beyond device lock |
| **AC-14** | Permitted Actions Without ID/Auth | §164.312(a)(2)(ii) Emergency Access | Break-glass access procedures with post-event accountability |
| **AU-10** | Non-Repudiation | §164.312(b) Audit Controls, §164.312(c)(2) | Cryptographic proof linking user identity to specific ePHI access events |
| **IA-3** | Device Identification and Authentication | §164.312(d) Person/Entity Auth | Machine-to-machine authentication for systems accessing ePHI |
| **SC-23** | Session Authenticity | §164.312(a)(2)(iii) Auto Logoff | Anti-session-hijacking controls |
| **SI-7** | Software and Information Integrity | §164.312(c) Integrity | Integrity verification of stored ePHI records |
| **SI-10** | Information Input Validation | §164.312(c)(2) Auth ePHI | Input validation preventing corruption of ePHI fields |

---

## Control Overlap Summary: HIPAA + ISO 27001 + CMMC Level 2

For completeness, several controls in this HIPAA mapping also appear in the CMMC Level 2 → NIST 800-53 mapping. Controls appearing in all three frameworks are the highest-value implementation targets for government-adjacent SaaS companies.

| NIST Control | HIPAA | ISO 27001 | CMMC L2 | Triple Coverage |
|---|---|---|---|---|
| AC-2 | §164.312(a)(1) | A.8.2, A.5.15 | AC.L2-3.1.1 | **★★★ TRIPLE** |
| AC-3 | §164.312(a)(1) | A.8.3 | AC.L2-3.1.1, 3.1.2 | **★★★ TRIPLE** |
| AC-6 | §164.312(a)(1) | A.8.2 | AC.L2-3.1.5, 3.1.6 | **★★★ TRIPLE** |
| AU-2 | §164.312(b) | A.8.15 | AU.L2-3.3.1 | **★★★ TRIPLE** |
| AU-3 | §164.312(b) | A.8.15 | AU.L2-3.3.1 | **★★★ TRIPLE** |
| AU-9 | §164.312(b)(c) | A.8.15, A.5.28 | AU.L2-3.3.2 | **★★★ TRIPLE** |
| IA-2 | §164.312(d) | A.8.5, A.5.16 | IA.L2-3.5.3 | **★★★ TRIPLE** |
| IA-5 | §164.312(d) | A.5.17, A.8.5 | IA.L2-3.5.7, 3.5.8 | **★★★ TRIPLE** |
| SC-8 | §164.312(e) | A.8.24 | SC.L2-3.13.8 | **★★★ TRIPLE** |
| SC-13 | §164.312(a)(2)(iv), §164.312(e)(2)(ii) | A.8.24 | SC.L2-3.13.10 | **★★★ TRIPLE** |
| SC-28 | §164.312(a)(2)(iv) | A.8.24, A.8.1 | SC.L2-3.13.16 | **★★★ TRIPLE** |

---

## NEOGOV Implementation Roadmap

### Phase 1 — No-Regrets Controls (implement first; satisfy all three frameworks)

These are the triple-coverage controls. Every dollar spent here satisfies HIPAA, ISO 27001, and CMMC simultaneously.

1. **Unique user IDs everywhere** (IA-2, IA-4, AC-2) — Eliminate all shared/generic accounts in production ePHI systems.
2. **MFA on all ePHI access** (IA-2(1), IA-2(6)) — Not just admin access; all users with access to HR/benefits data.
3. **Encryption at rest and in transit** (SC-8, SC-8(1), SC-13, SC-28) — RDS encryption, S3 encryption, TLS 1.3 enforced, certificate rotation documented.
4. **Audit logging with tamper protection** (AU-2, AU-3, AU-9, AU-12) — Application-layer logs to immutable storage; CloudWatch Logs with object lock.
5. **Least privilege access model** (AC-3, AC-6) — Role-based access control scoped to minimum ePHI access needed per function.

### Phase 2 — HIPAA-Specific Controls (required for BAA defensibility)

6. **Session management** (AC-11, AC-12, SC-23) — ≤15-minute idle timeout; explicit session termination on logout.
7. **Emergency access procedures** (AC-14, CP-2, IR-4) — Documented break-glass procedure with post-access audit review requirement.
8. **ePHI integrity verification** (SI-7, AU-10) — Checksums on exported data; non-repudiation for bulk access events.
9. **Device/entity authentication** (IA-3) — Certificate-based authentication for all integration partners and API clients sending ePHI.
10. **Input validation** (SI-10) — Validation on all ePHI field inputs at the API layer to prevent corruption.

### Phase 3 — Enhanced Assurance (for government agency customers requiring FISMA/FedRAMP alignment)

11. **FIPS 140-2/3 validated cryptographic modules** — Required for FISMA-covered agency customers; SC-13 with FIPS constraint.
12. **Phishing-resistant MFA** (IA-2(6)) — Hardware tokens or device-bound passkeys for all privileged access to ePHI systems.
13. **Continuous audit log review** (AU-6) — Automated SIEM alerting on unusual ePHI access patterns (bulk exports, after-hours access, new IP addresses).

---

## Breach Notification Decision Tree

Under 45 CFR §164.402, an impermissible use or disclosure of ePHI is a breach unless the covered entity/BA demonstrates a low probability of compromise using a 4-factor risk assessment. The Technical Safeguards above directly affect breach probability:

- **Encrypted ePHI** (SC-28, SC-13, SC-8(1)): If the lost/exposed data was encrypted and the key was not compromised, it qualifies for the encryption safe harbor — no breach notification required.
- **Audit controls present** (AU-2, AU-3, AU-9): Strong logging enables a defensible risk assessment demonstrating that impermissible access did/did not actually occur.
- **Access controls working** (AC-2, AC-3, IA-2): Demonstrates that unauthorized access was technically constrained, reducing probability of compromise.

**For NEOGOV:** Any incident involving ePHI must trigger a risk assessment using these controls as evidence. A well-documented implementation of Technical Safeguards is your first line of defense against mandatory breach notification and HHS enforcement.
