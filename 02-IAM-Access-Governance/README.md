
# Enterprise Identity Governance & Role-Based Access Control (RBAC) Matrix

> This artifact defines an enterprise Role-Based Access Control (RBAC) framework, Separation of Duties (SoD) conflict matrix, and Joiner-Mover-Leaver (JML) user lifecycle governance policy mapped to **NIST SP 800-53 Rev. 5** access control families.

---

### Part 1: Enterprise Role-Based Access Control (RBAC) Matrix

* **CRUD Permissions Key:** `C` = Create, `R` = Read, `U` = Update, `D` = Delete, `NA` = No Access
* **Administrative Scope:** `PIM` = Privileged Identity Management (Time-bound, JIT activation with approval)

| Enterprise Role | Department | Workday HRIS | NetSuite ERP / Billing | ServiceNow ITSM | Entra ID (P1/P2) | AWS Production Console |
| :--- | :--- | :---: | :---: | :---: | :---: | :---: |
| **HR Specialist** | Human Resources | `C / R / U` | `NA` | `R` (Self-service) | `NA` | `NA` |
| **Finance Analyst** | Finance & Accounting | `R` (Payroll only) | `C / R / U` | `R` (Tickets) | `NA` | `NA` |
| **IT Helpdesk Tech** | Information Technology | `R` (Employee Directory) | `NA` | `C / R / U / D` | `User Admin` (Tier 1) | `NA` |
| **Security Analyst (GRC/SOC)** | Information Security | `R` (Audit logs only) | `R` (Audit logs only) | `R / U` (Security Ops) | `Security Reader` | `Read-Only (CloudTrail)` |
| **Cloud Infrastructure Engineer** | IT Operations | `NA` | `NA` | `R / U` | `Global Reader` | `PowerUser (PIM Required)` |

---

### Part 2: Separation of Duties (SoD) Enforcement Matrix

To prevent fraud, error, and unmonitored lateral escalation, conflicting business and technical duties are structurally segregated:

| Primary Assigned Role | Conflicting Secondary Role | Operational Risk | Enforcement Mechanism |
| :--- | :--- | :--- | :--- |
| **Accounts Payable Clerk** | **Payment Disbursement Approver** | Creation and unilateral approval of fraudulent vendor invoices. | Hard barrier in NetSuite ERP; workflow enforces dual-authorization on transactions > $1,000. |
| **Software Developer** | **Production Release Admin** | Direct push of unreviewed, malicious, or non-compliant code into live environments. | Automated CI/CD branch protection; AWS IAM blocks developer credentials from deploying directly to production. |
| **IT System Administrator** | **Security / Compliance Auditor** | System admins erasing audit logs to conceal configuration tampering or unauthorized access. | Role separation in Entra ID; `Global Admin` accounts cannot modify or suppress `Log Analytics` repositories. |

---

### Part 3: Joiner-Mover-Leaver (JML) Lifecycle Workflow & SLAs
| Lifecycle Stage | Trigger Event | Identity Governance Workflow | Enforcement SLA |
| :--- | :--- | :--- | :---: |
| **Joiner (Onboarding)** | New employee record created in HRIS. | Automated provisioning of baseline birthright access (Email, Slack, SSO) via dynamic security groups. Role-specific access requests route through department manager approval. | Access active on Day 1 (08:00 EST). |
| **Mover (Internal Transfer)** | Department change or promotion in HRIS. | **Delta Access Review:** Manager approves new department roles. An automated revocation workflow purges all legacy group memberships and access rights within 48 hours to prevent **privilege creep**. | Within 48 hours of effective transfer date. |
| **Leaver (Scheduled Offboarding)** | Standard resignation or contract end date. | Scheduled deactivation: Entra account disabled, active browser sessions revoked via continuous access evaluation (CAE), mailboxes placed on legal hold. | Within 2 hours of end-of-day timestamp. |
| **Leaver (Adverse / Immediate)** | Urgent notification from HR / Legal / Security. | **Emergency Revocation:** Immediate account disablement, global session termination token revocation, and SSO credential purge across all federated apps. | **Within 15 minutes** of formal notice. |

---

### Part 4: NIST SP 800-53 Rev. 5 Control Mapping

| NIST Control ID | Control Name | Implementation Details in This Architecture |
| :--- | :--- | :--- |
| **AC-2** | Account Management | Automated account creation, maintenance, and deletion synced via Workday HRIS to Microsoft Entra ID. |
| **AC-3** | Access Enforcement | Access decisions enforced at the IdP level using Conditional Access and least-privilege role assignments. |
| **AC-5** | Separation of Duties | Formal policy separating development from production release, and accounting entry from disbursement. |
| **AC-6** | Least Privilege | Administrative roles require Just-in-Time (JIT) elevation via Privileged Identity Management (PIM) with 4-hour max lifetimes. |
| **IA-2** | Identification & Authentication | Mandatory FIDO2 / Phishing-Resistant MFA enforced for all workforce members; legacy basic authentication blocked. |
