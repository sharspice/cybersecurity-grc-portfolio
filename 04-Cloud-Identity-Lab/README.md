# Cloud Identity Governance & Conditional Access Lab (Microsoft Entra ID)

> This technical portfolio artifact documents the architecture, deployment, and audit testing of an enterprise Zero Trust identity infrastructure using **Microsoft Entra ID (Azure AD P2)**. The lab demonstrates real-world implementation of Conditional Access baseline policies, Privileged Identity Management (PIM) with Just-in-Time (JIT) access, and log monitoring aligned with **NIST SP 800-53 Rev. 5** and **NIST CSF 2.0**.

---

### Part 1: Environment & Architecture Overview

* **Directory Platform:** Microsoft Entra ID (Tenant: `Enterprise SecLab Sandbox`)
* **Licensing Tier:** Microsoft Entra ID P2 (Active features: Risk-Based Conditional Access, PIM, Access Reviews, Entitlement Management)
* **Architecture Strategy:** Zero Trust Baseline (*Explicit Verification, Least Privilege Access, Assume Breach*)

* ---

### Part 2: Implemented Conditional Access Policies (CAP Engine)

| Policy ID | Policy Name | Target Scope | Conditions & Signals | Access Grant Enforcement | NIST SP 800-53 Control |
| :--- | :--- | :--- | :--- | :--- | :---: |
| **CAP-01** | `Block-Legacy-Authentication` | All Users & Guests | Any Cloud Application via Basic Auth / Legacy Protocols (IMAP, POP3, SMTP, ActiveSync). | **BLOCK Access** (Prevents credential stuffing & password spray bypass). | `IA-2`, `AC-3` |
| **CAP-02** | `Require-MFA-Privileged-Admins` | `SG-Privileged-Admins` & High-Privilege Roles | All Cloud Management Consoles (Azure Portal, Entra Admin Center, M365 Admin). | **GRANT with Phishing-Resistant MFA** (FIDO2 / Authenticator Number Matching). | `IA-2(1)`, `AC-6` |
| **CAP-03** | `Enforce-Compliant-Device-Workforce` | All Corporate Workforce | Office 365, SharePoint, Exchange Online, Salesforce. | **GRANT with Intune Compliant Device** OR Hybrid Entra Joined state. | `AC-19`, `IA-3` |
| **CAP-04** | `Risk-Based-Session-Control` | All Users | Sign-in Risk: Medium or High (Evaluated via Identity Protection machine learning). | **Require Password Change via SSPR** with MFA verification. | `SI-4`, `SC-13` |

---

### Part 3: Privileged Identity Management (PIM) & JIT Governance

To eliminate persistent administrative attack surfaces, permanent directory assignments were replaced with time-bound Just-in-Time (JIT) role elevation.

| Monitored Entra Role | Assignment Type | Maximum Elevation Lifetime | Mandatory Activation Controls | Notification & Audit |
| :--- | :---: | :---: | :--- | :--- |
| **Global Administrator** | Eligible | **2 Hours** | Approver sign-off required (Security Lead), ticket reference, MFA step-up. | Real-time alert dispatched to SOC distribution list. |
| **Security Administrator** | Eligible | **4 Hours** | Phishing-resistant MFA prompt + documented business reason. | Logged in Entra Audit Logs (`PIM activation`). |
| **User Access Administrator** | Eligible | **4 Hours** | MFA step-up verification + ticket ID. | Logged in Entra Audit Logs (`PIM activation`). |

---

### Part 4: Threat Simulation, Audit Logs & Control Verification

#### 1. Legacy Authentication Attack Simulation
* **Scenario:** An external actor attempts unauthorized access via a script utilizing basic legacy authentication protocols (e.g., SMTP Auth) targeting `Sam.Finance`.
* **Telemetry Output:** Entra ID blocked the authentication attempt before evaluating credential validity.
* **Audit Log Evidence:**
  * `Result Type:` **Failure (53003 - Blocked by Conditional Access)**
  * `Applied Policy:` `CAP-01-Block-Legacy-Authentication`
  * `Client App:` `Authenticated SMTP`

#### 2. Privileged Role Elevation & Access Attestation
* **Scenario:** Analyst `Alex.SecOps` requested temporary elevation to `Security Administrator` to review tenant policy health.
* **Telemetry Output:**
  * PIM verified identity via FIDO2 token challenge.
  * System generated a 4-hour time-bound OAuth claim token.
  * Automated de-escalation confirmed at 04:00:00 elapsed time.

---

### Part 5: Compliance Framework & Control Mapping

| Framework Baseline | Framework Requirement | Implementation Method in Lab |
| :--- | :--- | :--- |
| **NIST CSF 2.0** | `PR.AA-01`: Identities are verified and bound to credentials. | Phishing-resistant MFA enforcement in `CAP-02`. |
| **NIST CSF 2.0** | `PR.AA-05`: Access permissions are managed according to least privilege. | PIM dynamic JIT role elevation replacing permanent assignments. |
| **NIST SP 800-53 Rev. 5** | `AC-2(2)`: Automated Account Removal / Temporary Access. | Time-decay role revocation upon PIM expiration. |
| **NIST SP 800-53 Rev. 5** | `AC-6(7)`: Review of Role-Based User Privileges. | Quarterly Entra Access Review attestations configured for all security groups. |
| **NIST SP 800-53 Rev. 5** | `AU-2 / AU-6`: Event Logging and Audit Record Review. | Ingestion and monitoring of Entra ID Sign-in and Audit telemetry. |
