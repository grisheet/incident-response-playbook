# Scenario 3: Data Breach

**Severity Range:** P2 (Limited exposure, internal data) to P1 (PII/PHI/PCI confirmed exfiltration)  
**MITRE ATT&CK Tactics:** Collection (TA0009), Exfiltration (TA0010), Command and Control (TA0011)  
**Primary MITRE Techniques:**
- T1530 — Data from Cloud Storage Object
- T1048 — Exfiltration Over Alternative Protocol
- T1567 — Exfiltration Over Web Service
- T1005 — Data from Local System
- T1213 — Data from Information Repositories

> **LEGAL NOTE:** Data breach response has strict regulatory timelines. Notify Legal immediately at P2+. GDPR requires notification within 72 hours of discovery if personal data is affected.

---

## Detection Methods

### Automated Detection
- [ ] DLP alert: Bulk data transfer of sensitive files (PII, financial records, health data)
- [ ] SIEM alert: Large data upload to unknown external IP or cloud storage (Dropbox, Mega.nz, pastebin)
- [ ] CASB alert: Anomalous cloud storage access (Microsoft Defender for Cloud Apps, Netskope)
- [ ] SIEM alert: Unusual database query volume (10x+ normal) by single user/service account
- [ ] EDR alert: Staging behavior — archiving/compressing files before exfiltration (7zip, WinRAR on sensitive folders)
- [ ] Network alert: Encrypted data transfer to non-whitelisted external domains
- [ ] SIEM alert: Privileged account accessing data stores outside normal hours
- [ ] Data warehouse alert: Abnormal API calls or data export volume

### External Notification Sources
- [ ] Third-party notification: Security researcher reports exposed data online
- [ ] Dark web monitoring alert: Company credentials or data found on breach forums
- [ ] Law enforcement notification of company data found in investigation
- [ ] Customer reports: receiving unauthorized emails with their data
- [ ] Media inquiry: journalist reports of company data exposure

---

## Triage Steps

**Assign Severity:**
- P2: Sensitive internal data accessed without authorization, no confirmed external exfiltration
- P1: PII/PHI/PCI/IP confirmed exfiltrated to external destination

**Initial Triage Questions (SOC L1):**
- [ ] What type of data was accessed/exfiltrated? (PII, PHI, PCI, IP, credentials)
- [ ] How many records/individuals are potentially affected?
- [ ] Was data exfiltrated externally or only accessed internally?
- [ ] Who (user account or system) accessed the data?
- [ ] When did the breach begin? (First unauthorized access timestamp)
- [ ] Is exfiltration ongoing or completed?
- [ ] What data repositories were accessed? (Database, S3 bucket, SharePoint, file server)
- [ ] Is there any indication of external threat actor vs. insider?

**Immediate Evidence Collection:**
- [ ] Capture database query logs with timestamps and user identifiers
- [ ] Collect DLP logs showing what files/records were accessed or transmitted
- [ ] Preserve network flow data showing exfiltration destination IP/domain
- [ ] Capture CASB logs for cloud storage access anomalies
- [ ] Screenshot and preserve any alerts in original state (chain of custody)
- [ ] Identify all data stores touched by the compromised account/system
- [ ] Collect IAM/access logs from cloud providers (AWS CloudTrail, Azure Monitor)
- [ ] Preserve email logs if data was sent via email

---

## Containment Strategy

### Immediate Containment (0–30 minutes)
- [ ] **Stop active exfiltration:** Block destination IP/domain at firewall, proxy, CASB
- [ ] **Disable compromised account(s)** if insider or account takeover is suspected
- [ ] **Revoke API keys/tokens** used in unauthorized data access
- [ ] **Lock exposed cloud storage buckets/containers** (set to private, disable public access)
- [ ] **Block unauthorized application** from further data access (CASB policy, OAuth revoke)
- [ ] **Enable enhanced logging** on all accessed data repositories
- [ ] **Notify Legal and CISO** immediately — regulatory clock may have started

### Short-Term Containment (30 min – 4 hours)
- [ ] Conduct access audit: Who else has access to the breached data store?
- [ ] Review and tighten IAM permissions on affected repositories
- [ ] Search for additional exfiltration channels (USB, email, cloud sync)
- [ ] Identify if attacker still has persistence in the environment
- [ ] Preserve all affected data stores for forensic analysis (do not delete or modify)
- [ ] Determine if data was publicly exposed (open S3 bucket, unsecured API endpoint)

### Long-Term Containment (4–24 hours)
- [ ] Comprehensive IAM audit across all data stores
- [ ] Review and enforce least-privilege access policies
- [ ] Deploy additional DLP rules based on observed exfiltration technique
- [ ] Conduct dark web monitoring for company data

---

## Eradication Steps

- [ ] Remove attacker access: terminate sessions, revoke all tokens, reset credentials
- [ ] Close the vulnerability that enabled unauthorized access (misconfigured S3, unpatched API, stolen credentials)
- [ ] Remove any data exfiltration tools or scripts left by attacker
- [ ] Rotate all credentials and API keys with potential exposure
- [ ] Remediate misconfigured storage permissions (S3 bucket policies, Azure Blob ACLs)
- [ ] Patch exploited vulnerability (CVE) if applicable
- [ ] Remove any backdoor access mechanisms (malicious Lambda functions, rogue IAM roles)
- [ ] Verify attacker has no remaining access to any systems

---

## Recovery Procedures

- [ ] Restore legitimate access for affected users after credential rotation
- [ ] Re-enable data repositories after access controls are verified
- [ ] Validate data integrity — confirm no data tampering occurred
- [ ] Monitor all data access for 30 days post-incident (enhanced logging)
- [ ] Conduct access review — remove any unnecessary privileges discovered during investigation
- [ ] Verify DLP policies are catching similar patterns
- [ ] Communicate with affected departments about resuming normal operations

---

## Regulatory Notification Requirements

| Regulation | Notification Deadline | Who to Notify | Trigger Condition |
|-----------|----------------------|---------------|-------------------|
| **GDPR** | 72 hours from discovery | Supervisory Authority + individuals if high risk | Personal data of EU residents |
| **HIPAA** | 60 days from discovery (individual), annual for <500 | HHS + individuals | PHI of US patients |
| **PCI DSS** | Immediately | Card brands + acquiring bank | Payment card data |
| **CCPA** | As expeditiously as possible | California residents | CA resident personal info |
| **SEC** | 4 business days (material incidents) | SEC (Form 8-K) | Publicly traded companies |
| **State Laws** | Varies (30–90 days) | State AG + affected residents | Varies by state |

---

## Post-Incident Actions

- [ ] **Determine scope of affected individuals:** compile complete list for notification
- [ ] **Legal review** of notification requirements with outside counsel
- [ ] **Draft breach notification letters** for affected individuals
- [ ] **Establish call center/support line** for affected individuals
- [ ] **Submit regulatory notifications** per the table above
- [ ] **Credit monitoring services** for affected individuals if financial data involved
- [ ] **Post-mortem report** within 5 business days: root cause, timeline, impact quantification
- [ ] **Architecture review:** least privilege enforcement, data classification, encryption at rest
- [ ] **Update DLP policies** and CASB rules based on observed TTPs
- [ ] **Cyber insurance notification** and claim documentation
- [ ] **Third-party notification** if vendor/partner data was compromised

---

## MITRE ATT&CK Mapping

| Phase | Technique ID | Technique Name | Detection Source |
|-------|-------------|----------------|------------------|
| Discovery | T1083 | File and Directory Discovery | EDR, SIEM |
| Discovery | T1213 | Data from Information Repositories | DLP, SIEM |
| Collection | T1005 | Data from Local System | DLP, EDR |
| Collection | T1530 | Data from Cloud Storage | CASB, CloudTrail |
| Collection | T1114 | Email Collection | Email Logs, CASB |
| Exfiltration | T1048.003 | Exfiltration Over Unencrypted Protocol | DLP, Network |
| Exfiltration | T1567.002 | Exfiltration to Cloud Storage | CASB, DLP |
| Exfiltration | T1041 | Exfiltration Over C2 Channel | Network, SIEM |
| Command & Control | T1071 | Application Layer Protocol | Network, Proxy |

---

## Automation Opportunities

| Step | Automation | Tool |
|------|-----------|------|
| Bulk access detection | ML-based anomaly detection on query volume | Microsoft Sentinel, Splunk UBA |
| Exfiltration blocking | Auto-block destination IP on DLP high-severity alert | Forcepoint, Cortex XSOAR |
| Account suspension | Auto-suspend account on confirmed data exfiltration | Azure Logic Apps, Okta Workflow |
| Regulatory timeline | Auto-create regulatory notification task on P1 | ServiceNow, Jira |
| Dark web monitoring | Automated scan for company data on breach forums | Recorded Future, Digital Shadows |
| Evidence preservation | Auto-snapshot cloud logs on breach detection | AWS Lambda, Azure Function |

---

*Back to [Main Playbook](../INCIDENT_RESPONSE_PLAYBOOK.md)*
