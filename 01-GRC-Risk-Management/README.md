# Enterprise Risk Register (NIST CSF 2.0)

> This enterprise risk register catalogs realistic threat vectors across the NIST CSF 2.0 framework, calculating inherent risk against quantitative likelihood-impact scoring and demonstrating risk reduction through NIST SP 800-53 compensating controls.

---

### Enterprise Risk Register Matrix

| Risk ID | NIST CSF 2.0 Function | Threat Scenario & Description | Inherent Score (L × I) | Risk Treatment | Compensating Control (NIST SP 800-53) | Residual Score (L × I) |
| :--- | :--- | :--- | :---: | :--- | :--- | :---: |
| **RSK-001** | Protect (`PR.AA`) | Credential compromise via spear-phishing accessing payroll/HR records. | **16** (4 × 4) | Mitigate | Enforce FIDO2 phishing-resistant MFA via Entra ID, disable legacy auth, run quarterly phishing drills (*IA-2, AT-2*). | **3** (1 × 3) |
| **RSK-002** | Protect (`PR.PS`) | Ransomware execution on endpoints exploiting unpatched client software. | **20** (4 × 5) | Mitigate | Centralized patch management enforcing updates within 72 hrs; EDR agent with automated host isolation (*SI-2, SI-4*). | **4** (1 × 4) |
| **RSK-003** | Govern (`GV.SC`) | Customer PII exposure via breach at third-party SaaS cloud billing provider. | **15** (3 × 5) | Mitigate / Transfer | Mandatory annual vendor risk assessments, SOC 2 Type II reviews, and 24-hr breach notification SLAs (*SA-9, SR-3*). | **6** (2 × 3) |
| **RSK-004** | Protect (`PR.AA`) | Privilege creep and unauthorized access from unrevoked transferred employee permissions. | **12** (4 × 3) | Mitigate | Automated Joiner-Mover-Leaver (JML) access reviews via Entra ID PIM with time-bound role activation (*AC-2, AC-6*). | **3** (1 × 3) |
| **RSK-005** | Recover (`RC.RP`) | Extended downtime/data loss following disaster due to untested backup procedures. | **10** (2 × 5) | Mitigate | 3-2-1 backup architecture with air-gapped immutable storage and bi-annual automated restoration drills (*CP-9, CP-10*). | **4** (1 × 4) |

---

### Key Competencies Demonstrated
* **Framework Alignment:** NIST CSF 2.0 (`Govern`, `Protect`, `Detect`, `Recover`)
* **Control Baseline:** NIST SP 800-53 Rev. 5 (`AC`, `IA`, `SI`, `SA`, `CP` control families)
* **Risk Scoring Model:** 5×5 Qualitative Matrix ($Likelihood \times Impact = Inherent / Residual\ Score$)
* **Risk Treatment Strategies:** Mitigation, Acceptance, Avoidance, and Transfer
