# Third-Party Risk Assessment (TPRA) & Vendor Security Review

> This artifact documents an enterprise Third-Party Risk Assessment (TPRA) evaluating a Tier-1 critical SaaS vendor. It analyzes a SOC 2 Type II audit report, assesses control exceptions, evaluates compensating controls, and establishes a conditional approval framework aligned with **NIST SP 800-53 Rev. 5** and **NIST CSF 2.0 (Govern - Supply Chain Risk Management)**.

---

### Part 1: Vendor & Engagement Profile

| Assessment Attribute | Vendor Detail |
| :--- | :--- |
| **Vendor Name** | CloudMetrics Analytics Inc. |
| **Service Evaluated** | Enterprise Cloud Data Analytics & Customer Telemetry Platform (SaaS) |
| **Data Classification Handled** | **Confidential / Restricted** (Customer PII, Hashed Account IDs, System Telemetry) |
| **Hosting Infrastructure** | Amazon Web Services (AWS) - US-East (N. Virginia) & US-West (Oregon) |
| **Business Impact Rating** | **Tier 1 (High Criticality)** - Disruption halts customer data analytics; breach exposes customer PII. |
| **Assessment Type** | Annual Vendor Security Review / Renewal Assessment |

---

### Part 2: Security & Compliance Evaluation Matrix

The evaluation reviewed CloudMetrics' latest **SOC 2 Type II Report** (Audit Period: October 1, 2025 – September 30, 2026, issued by Ernst & Young LLP), ISO/IEC 27001:2022 certificate, and completed **CAIQ (Consensus Assessments Initiative Questionnaire)**.

| Trust Services Criteria / Domain | Audit Finding / Control Posture | Assessment Status | GRC Analyst Analysis |
| :--- | :--- | :---: | :--- |
| **Common Criteria (Security)** | Enforces TLS 1.3 in transit and AES-256 at rest via AWS KMS with customer-managed keys. | **Compliant** | Encryption baseline meets FIPS 140-3 and corporate data protection standards. |
| **Access Control (IAM)** | Phishing-resistant MFA enforced via Okta SSO; JIT role elevation via PAM for production access. | **Compliant** | Least privilege enforced; no broad standing administrative privileges detected in production. |
| **System Availability** | Multi-AZ deployment across AWS regions; documented RPO = 1 hour, RTO = 4 hours. | **Compliant** | DR restoration tests executed semi-annually; evidence verified in SOC 2 Section IV. |
| **Confidentiality & Privacy** | Documented data retention/purge workflows; sub-processor agreements (DPA) execute standard contractual clauses. | **Compliant** | Compliant with GDPR/CCPA data subject deletion request SLAs. |

---

### Part 3: Identified SOC 2 Type II Control Exceptions & Risk Analysis

The independent auditor noted two control exceptions during the 12-month testing period. Below is the technical risk analysis and evaluation of vendor compensating controls:

| Exception ID | SOC 2 Control Reference | Auditor Finding / Description | Inherent Severity | Vendor Compensating Control | Residual Severity |
| :--- | :--- | :--- | :---: | :--- | :---: |
| **EXC-01** | `CC6.2: User Offboarding` | 3 of 45 sampled terminated employee accounts retained active SSO access for up to 72 hours post-termination. | **Medium** | Automated SCIM deprovisioning was implemented in Q3 2026; audit logs confirmed zero unauthorized logins occurred from these accounts. | **Low** |
| **EXC-02** | `CC7.1: Vulnerability Patching` | 2 medium-severity CVEs on internal monitoring nodes exceeded the 30-day internal SLA by 14 days. | **Low** | Affected nodes are deployed in isolated private subnets with no ingress internet access and protected by AWS Network Firewall. | **Low** |

---

### Part 4: NIST SP 800-53 Rev. 5 Supply Chain Control Mapping

| NIST Control ID | Control Name | Vendor Verification & Contractual Requirement |
| :--- | :--- | :--- |
| **SA-9** | External System Services | Vendor contractually mandated to undergo annual independent SOC 2 Type II audits and supply remediation letters for exceptions. |
| **SR-3** | Supply Chain Controls & Processes | Continuous monitoring of vendor sub-processors; vendor must notify within 30 days of onboarding new sub-service organizations. |
| **SR-5** | Acquisition Strategies & Tools | Enforced data return and cryptographic erasure verification upon contract termination or service decommissioning. |
| **IR-6** | Incident Reporting | Mandatory contractual requirement to notify the enterprise security operations center within **24 hours** of a confirmed security incident. |

---

### Part 5: GRC Analyst Decision & Sign-Off Recommendation

* **Final Disposition:** **CONDITIONAL APPROVAL**
* **Approval Validity Period:** 12 Months (Valid through September 2027)

**Conditions of Approval:**
1. **Remediation Attestation (90-Day SLA):** CloudMetrics must provide quarterly SCIM deprovisioning audit logs by November 30, 2026, confirming 100% adherence to same-day access revocation.
2. **Contractual Security Addendum:** Legal must ensure the Master Services Agreement (MSA) includes the updated **24-hour security incident notification clause** and mandatory cyber liability insurance coverage ($5M minimum).
3. **Continuous Monitoring:** The Third-Party Risk team will maintain automated threat intelligence monitoring on CloudMetrics' external attack surface via SecurityScorecard.
