# CMMC Level 2 → NIST 800-171 Rev 2 → NIST 800-53 Rev 5 Master Mapping

**Author:** Shayl Taveras
**Date:** 2026-03-03
**Version:** 1.0

> CMMC Level 2 is a direct 1:1 overlay of all 110 NIST 800-171 Rev 2 requirements.

---

## Domain Summary Table

| # | Domain | Practices | 800-53 Rev 5 Families | Weight |
|---|---|---|---|---|
| 1 | Access Control | 22 | AC, CM-7, PE-3 | Heaviest domain |
| 2 | Awareness & Training | 3 | AT | Light |
| 3 | Audit & Accountability | 9 | AU, SI-12 | Moderate |
| 4 | Configuration Management | 9 | CM, SA-10 | Moderate |
| 5 | Identification & Authentication | 11 | IA | High |
| 6 | Incident Response | 3 | IR | Light |
| 7 | Maintenance | 6 | MA | Moderate |
| 8 | Media Protection | 9 | MP | Moderate |
| 9 | Personnel Security | 2 | PS | Lightest |
| 10 | Physical Protection | 6 | PE | Moderate |
| 11 | Risk Assessment | 3 | RA | Light |
| 12 | Security Assessment | 4 | CA | Light |
| 13 | System & Comms Protection | 16 | SC, AC-4, AC-17 | High |
| 14 | System & Info Integrity | 7 | SI, RA-5 | Moderate |
| | **Total** | **110** | | |

---

## Detailed Domain Mappings

### 1. Access Control (AC)

**Practices:** 22 (3.1.1 – 3.1.22)
**NIST 800-53 Rev 5 Families:** AC, CM-7 (least functionality), PE-3 (physical access tie-in)
**CMMC Note:** CMMC scoping guidance explicitly requires CUI boundary documentation before AC
controls can be assessed — a step 800-53 leaves to the organization's discretion and often
skipped in self-assessments.

---

### 2. Awareness and Training (AT)

**Practices:** 3 (3.2.1 – 3.2.3)
**NIST 800-53 Rev 5 Families:** AT
**CMMC Note:** CMMC requires insider threat awareness training specifically by name under 3.2.2,
whereas 800-53 AT-2 treats insider threat awareness as an enhancement (AT-2(2)) rather than a
baseline requirement.

---

### 3. Audit and Accountability (AU)

**Practices:** 9 (3.3.1 – 3.3.9)
**NIST 800-53 Rev 5 Families:** AU, SI-12 (information management)
**CMMC Note:** 3.3.5 requires correlation of audit records across multiple sources — 800-53
treats this as AU-12(3), an enhancement, but CMMC makes it a flat Level 2 requirement with no
enhancement designation.

---

### 4. Configuration Management (CM)

**Practices:** 9 (3.4.1 – 3.4.9)
**NIST 800-53 Rev 5 Families:** CM, SA-10 (developer config management)
**CMMC Note:** 3.4.9 requires controlling and monitoring user-installed software, which CMMC
assessors routinely flag as a gap in cloud-native environments where users have broad install
privileges through SaaS app stores or package managers.

---

### 5. Identification and Authentication (IA)

**Practices:** 11 (3.5.1 – 3.5.11)
**NIST 800-53 Rev 5 Families:** IA
**CMMC Note:** 3.5.3 mandates MFA for all access to organizational systems — CMMC assessors
apply this to privileged and non-privileged access alike, whereas many 800-53 implementations
limit MFA enforcement to IA-2(1) (privileged accounts only) at moderate baseline.

---

### 6. Incident Response (IR)

**Practices:** 3 (3.6.1 – 3.6.3)
**NIST 800-53 Rev 5 Families:** IR
**CMMC Note:** 3.6.2 explicitly requires incident reporting to DoD (US-CERT/CISA) within 72
hours — a contractual and regulatory overlay that goes beyond what 800-53 IR-6 specifies at the
moderate baseline, which leaves reporting timelines undefined.

---

### 7. Maintenance (MA)

**Practices:** 6 (3.7.1 – 3.7.6)
**NIST 800-53 Rev 5 Families:** MA
**CMMC Note:** 3.7.5 requires MFA for remote maintenance sessions specifically — in cloud
environments where all maintenance is remote by definition, this effectively mandates MFA on all
administrative sessions, closing a gap that many cloud-first organizations leave open.

---

### 8. Media Protection (MP)

**Practices:** 9 (3.8.1 – 3.8.9)
**NIST 800-53 Rev 5 Families:** MP
**CMMC Note:** 3.8.7 restricts use of removable media without organizational identification —
CMMC assessors increasingly apply this to virtual media and cloud storage endpoints, not just
physical USB drives, expanding scope beyond traditional 800-53 MP-7 interpretations.

---

### 9. Personnel Security (PS)

**Practices:** 2 (3.9.1 – 3.9.2)
**NIST 800-53 Rev 5 Families:** PS
**CMMC Note:** Despite having only 2 practices, CMMC assessors routinely cite 3.9.2 (termination
and transfer procedures) as a gap because organizations fail to document CUI access revocation
separately from standard IT offboarding — 800-53 PS-4 does not make this distinction explicit.

---

### 10. Physical Protection (PE)

**Practices:** 6 (3.10.1 – 3.10.6)
**NIST 800-53 Rev 5 Families:** PE
**CMMC Note:** CMMC scoping guidance clarifies that cloud service providers must have a FedRAMP
authorization at the appropriate impact level for PE controls to be inherited — unlike 800-53,
CMMC does not allow informal inheritance without a formal authorization boundary documented in
an SSP.

---

### 11. Risk Assessment (RA)

**Practices:** 3 (3.11.1 – 3.11.3)
**NIST 800-53 Rev 5 Families:** RA
**CMMC Note:** 3.11.2 requires periodic vulnerability scanning — CMMC guidance specifies that
scanning must cover the full CUI boundary and that results must be remediated within defined
timelines, a specificity that 800-53 RA-5 defers to organizational policy.

---

### 12. Security Assessment (CA)

**Practices:** 4 (3.12.1 – 3.12.4)
**NIST 800-53 Rev 5 Families:** CA
**CMMC Note:** 3.12.4 requires a System Security Plan documenting all 110 practices — CMMC
assessors use the SSP as the primary evidence artifact and will find a deficiency if any
requirement lacks a documented implementation statement, a rigor that exceeds most 800-53
moderate baseline implementations.

---

### 13. System and Communications Protection (SC)

**Practices:** 16 (3.13.1 – 3.13.16)
**NIST 800-53 Rev 5 Families:** SC, AC-4 (information flow), AC-17 (remote access)
**CMMC Note:** 3.13.16 requires protecting CUI at rest — CMMC explicitly scopes this to include
data in cloud storage, backup systems, and log archives, whereas 800-53 SC-28 at moderate
baseline is frequently implemented with a narrower scope that excludes backup and archival
systems.

---

### 14. System and Information Integrity (SI)

**Practices:** 7 (3.14.1 – 3.14.7)
**NIST 800-53 Rev 5 Families:** SI, RA-5 (vulnerability monitoring)
**CMMC Note:** 3.14.6 and 3.14.7 require monitoring organizational systems and environments for
attacks — CMMC assessors expect documented detection capability with evidence of alerts and
response, not just tool deployment, which goes beyond what 800-53 SI-4 requires at moderate
baseline in terms of operationalized evidence.

---

## Hardest CMMC Domains for a SaaS Company

### 1. Physical Protection (PE) — 6 practices

SaaS companies run on AWS, Azure, or GCP and have zero physical control over the hardware their
systems run on. CMMC requires the cloud provider to hold a FedRAMP authorization at the
appropriate impact level before PE controls can be inherited. Running on commercial AWS (not
GovCloud) means PE controls cannot be inherited at all — forcing SaaS companies to either
migrate to GovCloud/Azure Government or document compensating controls for an environment they
do not physically control.

### 2. Configuration Management (CM) — 9 practices

SaaS environments are built for speed. Continuous deployment pipelines push changes multiple
times per day and infrastructure is ephemeral. This creates three specific gaps:

- **3.4.1 (baseline configurations):** Hard to define for container-based infrastructure when
  images rebuild constantly
- **3.4.3 (change control):** Every PR merge is a change to a production system — CMMC expects
  a documented change control process, not just a git log
- **3.4.9 (user-installed software):** Developer machines with unrestricted package manager
  access violate this practice immediately

### 3. System and Communications Protection (SC) — 16 practices

Multi-tenancy is the business model of SaaS — and it is architecturally in tension with CUI
isolation. Achieving SC compliance often requires a dedicated CUI-isolated environment — a
GovCloud tenant with separate database clusters, separate logging infrastructure, and separate
deployment pipelines — representing 30–50% of a SaaS company's re-architecture effort.

---

## SPRS Scoring System

### Score Range: -203 to 110

The Supplier Performance Risk System (SPRS) score is calculated using the DoD Assessment
Methodology:

- **Starting point:** 110 (assume full compliance)
- **NOT MET:** subtract the requirement's assigned point value
- **PARTIALLY MET:** subtract half the point value (rounded up)
- **Minimum score:** -203 (zero requirements implemented)
- **Total scoring range:** 313 points (110 + 203)

### Point Weight Distribution

| Weight | Control Characteristics |
|---|---|
| **5 points** | Technical controls with direct CUI exposure impact — MFA, encryption, audit logging |
| **3 points** | Operational controls — access reviews, incident response, config baselines |
| **1 point** | Documentation and procedural controls — policies, training records, plans |

### Highest Impact Controls

| Requirement | Practice | Weight |
|---|---|---|
| 3.5.3 | Multi-factor authentication for all system access | 5 |
| 3.13.8 | Encrypt CUI during transmission | 5 |
| 3.13.16 | Protect CUI at rest | 5 |
| 3.3.1 | Create and retain audit logs | 5 |
| 3.3.2 | Ensure audit logs are reviewable | 5 |
| 3.1.2 | Limit system access to authorized transactions | 5 |
| 3.4.2 | Establish security configuration baselines | 3 |
| 3.11.2 | Perform periodic vulnerability scans | 3 |
| 3.12.4 | Develop and maintain a System Security Plan | 3 |

### SPRS Score Thresholds

| Score | Interpretation |
|---|---|
| **110** | Full compliance — all 110 practices implemented |
| **88+** | Strong posture — typical for mature DoD contractors |
| **60–87** | Moderate gaps — POA&M expected |
| **Below 60** | Significant risk — DCSA/DIBCAC medium or high assessment likely |
| **Negative** | Systemic failure — contractual risk, potential loss of award |
| **-203** | No controls implemented |

### The SSP as a Gate Control

The SSP (3.12.4) functions as a gate control in CMMC assessments. If an assessor cannot find a
documented implementation statement for a practice in the SSP, they will mark it NOT MET
regardless of whether the technical control is implemented. Unlike FedRAMP, there is no
opportunity to remediate during the assessment — the SSP must be complete and accurate before
day one of the assessment.

---

*Reference: CMMC Model v2.0 | NIST SP 800-171 Rev 2 | NIST SP 800-53 Rev 5 | DoD Assessment Methodology v1.2.1*
