# Scenario 4: Insider Threat

**Severity Range:** P3 (Policy violation, suspicious behavior) to P1 (Confirmed data theft, sabotage)  
**MITRE ATT&CK Tactics:** Collection (TA0009), Exfiltration (TA0010), Impact (TA0040), Defense Evasion (TA0005)  
**Primary MITRE Techniques:**
- T1078 — Valid Accounts
- T1052 — Exfiltration Over Physical Medium (USB)
- T1567 — Exfiltration Over Web Service
- T1485 — Data Destruction
- T1070 — Indicator Removal

> **HR & LEGAL CRITICAL:** Insider threat investigations require immediate coordination with HR, Legal, and potentially Law Enforcement. Evidence must be handled with strict chain of custody. Do not confront or alert the subject. Actions must comply with employment law and privacy regulations.

---

## Insider Threat Categories

| Type | Description | Example |
|------|-------------|--------|
| **Malicious Insider** | Deliberate unauthorized action for personal gain or sabotage | Disgruntled employee stealing IP before resignation |
| **Negligent Insider** | Unintentional policy violation causing security risk | Employee storing sensitive data on personal cloud storage |
| **Compromised Insider** | Legitimate account taken over by external threat actor | Attacker using stolen employee credentials |
| **Colluding Insider** | Employee working with external threat actor | Employee providing access credentials to competitor |

---

## Detection Methods

### Behavioral Analytics (UEBA)
- [ ] UEBA alert: User accessing 10x+ normal data volume
- [ ] UEBA alert: Access to sensitive data repositories outside normal working hours
- [ ] UEBA alert: Accessing systems/data not related to job role
- [ ] UEBA alert: Anomalous peer comparison — accessing data no colleagues in same role access
- [ ] SIEM alert: Account accessing data from unusual location or device
- [ ] UEBA alert: Multiple failed access attempts to restricted systems

### Technical Indicators
- [ ] DLP alert: Large file upload to personal cloud storage (Dropbox, Google Drive personal, OneDrive personal)
- [ ] DLP alert: Bulk email forwarding to personal email address
- [ ] Endpoint alert: Mass USB file copy activity
- [ ] SIEM alert: Printing large volumes of sensitive documents
- [ ] EDR alert: Disabling or tampering with endpoint security tools
- [ ] SIEM alert: Privilege escalation attempts or accessing admin tools not in job scope
- [ ] Database alert: Unusual bulk SELECT queries or data export attempts
- [ ] Email DLP: Sensitive document attached to personal email domain

### HR/Contextual Indicators (Coordinate with HR)
- [ ] Employee on Performance Improvement Plan (PIP) with sudden increased data access
- [ ] Employee resigned/terminated and still has active access (separation checklist failure)
- [ ] Employee accessing competitor websites or job boards from corporate device
- [ ] Reports from colleagues of suspicious behavior
- [ ] Financial stress indicators or recent disciplinary action

---

## Triage Steps

**Assign Severity:**
- P4: Policy violation, no sensitive data involved, no malicious intent apparent
- P3: Suspicious behavior, sensitive data accessed anomalously, intent unclear
- P2: Confirmed unauthorized data access, possible exfiltration, limited scope
- P1: Confirmed data theft, sabotage, or active system compromise by insider

**CRITICAL: Notify Before Acting**
1. Immediately notify **CISO**
2. Notify **Legal Counsel**
3. Notify **HR** (if employee-related)
4. Do NOT alert or confront the subject
5. Engage **Law Enforcement** if criminal activity is suspected (P1)

**Initial Triage Questions:**
- [ ] Who is the subject? Employee, contractor, vendor, or ex-employee?
- [ ] What data was accessed, copied, or exfiltrated?
- [ ] What is the business impact of the data involved (sensitivity classification)?
- [ ] Is this behavior consistent with the user's job role?
- [ ] Is the employee currently active, on leave, or departed?
- [ ] Are there any recent HR events (PIP, termination notice, disciplinary action)?
- [ ] What technical evidence is available? (DLP, UEBA, logs)

**Evidence Collection (Chain of Custody Required):**
- [ ] Preserve UEBA/SIEM alerts without modification
- [ ] Collect DLP logs with timestamps of data access and transfer events
- [ ] Capture network logs showing destination of transferred data
- [ ] Forensic image of employee endpoint (must be authorized by Legal/HR)
- [ ] Preserve email logs (sent/received) for the subject
- [ ] Collect badge access logs (physical access records)
- [ ] Screenshot and log all evidence with timestamps
- [ ] Document every step taken with time, action, and responsible analyst
- [ ] Store all evidence in secure, access-controlled location

---

## Containment Strategy

> **NOTE:** Containment must be carefully timed and coordinated with HR and Legal to avoid tipping off the subject prematurely and to ensure employment law compliance.

### Covert Phase (Do Not Alert Subject)
- [ ] Enable enhanced monitoring on subject's accounts and devices (coordinate with Legal)
- [ ] Enable verbose logging on all systems subject has access to
- [ ] Place DLP policy in monitor-only mode (vs. block) to capture ongoing evidence without alerting subject
- [ ] Deploy honeypot data: place tagged decoy documents in paths subject has accessed
- [ ] Preserve logs in isolated, read-only storage to prevent tampering

### Overt Containment (Coordinated with HR — When Authorized)
- [ ] Disable subject's account(s) in Active Directory / Azure AD
- [ ] Revoke all SSO sessions, VPN access, and MFA tokens
- [ ] Revoke remote access (VPN, Citrix, RDP)
- [ ] Recover corporate devices (coordinate with HR for physical retrieval)
- [ ] Change passwords for any shared/service accounts the subject had access to
- [ ] Disable physical access card / building access
- [ ] Notify IT Help Desk and physical security simultaneously during overt phase

### Scope Assessment
- [ ] Enumerate all systems, data stores, and applications the subject accessed in the investigation window
- [ ] Identify all accounts (including service accounts) the subject had access to
- [ ] Determine if subject shared access with external parties
- [ ] Review subject's email and collaboration history for anomalous communications

---

## Eradication Steps

- [ ] Revoke all access permanently — all accounts, applications, and physical facilities
- [ ] Remove subject from all Active Directory groups and role assignments
- [ ] Rotate credentials for all shared accounts and systems the subject had access to
- [ ] Remove any backdoors or access mechanisms subject may have created
- [ ] Review and remove any scheduled tasks, scripts, or automation the subject created
- [ ] Verify no data destruction or tampering has occurred
- [ ] Recover and quarantine any exfiltrated data if possible (legal process may be required)
- [ ] Confirm no accomplices have remaining access

---

## Recovery Procedures

- [ ] Re-assign subject's responsibilities and access rights to other employees as appropriate
- [ ] Verify data integrity in repositories the subject had write access to
- [ ] Review and update access controls to enforce least-privilege
- [ ] Ensure no business-critical processes depended solely on the subject (single points of failure)
- [ ] Implement separation of duties for sensitive processes the subject was sole owner of
- [ ] Communicate to affected teams as appropriate (on need-to-know basis)
- [ ] Monitor for any delayed impact — logic bombs, time-delayed sabotage

---

## Legal & HR Considerations

| Consideration | Action Required |
|--------------|----------------|
| Evidence Chain of Custody | Document every evidence action with timestamp, analyst, and justification |
| Employee Privacy Rights | Consult Legal before accessing personal devices or personal email |
| Wrongful Termination Risk | HR must follow proper termination procedures even in confirmed cases |
| Criminal Prosecution | Engage Legal to decide whether to involve law enforcement (FBI, local) |
| Civil Litigation | Preserve all evidence for potential civil recovery action |
| Non-Disclosure Agreements | Review NDAs and IP assignment agreements for violation documentation |
| Regulatory Reporting | Determine if breach triggers regulatory notification requirements |

---

## Post-Incident Actions

- [ ] **Complete investigation report** with full timeline, evidence summary, and impact assessment
- [ ] **HR action:** Formal disciplinary or termination process (coordinated with Legal)
- [ ] **Law enforcement referral** if criminal activity confirmed (trade secret theft, sabotage)
- [ ] **Civil recovery:** Engage attorneys for potential lawsuit for damages or injunction
- [ ] **Lessons learned:** What access controls failed? What allowed the insider to go undetected?
- [ ] **Access control review:** Implement least-privilege across all systems
- [ ] **Offboarding process review:** Ensure separation checklist removes all access immediately
- [ ] **UEBA tuning:** Update behavioral baselines and alert thresholds based on observed patterns
- [ ] **Security culture assessment:** Anonymous employee survey, ethics hotline promotion
- [ ] **Tabletop exercise** simulating insider threat scenario for IR team
- [ ] **Update employee handbook** with clear acceptable use and data handling policies

---

## MITRE ATT&CK Mapping

| Phase | Technique ID | Technique Name | Detection Source |
|-------|-------------|----------------|------------------|
| Initial Access | T1078 | Valid Accounts | SIEM, UEBA |
| Collection | T1005 | Data from Local System | DLP, EDR |
| Collection | T1213 | Data from Information Repositories | DLP, SIEM |
| Collection | T1114.001 | Local Email Collection | Email Logs |
| Exfiltration | T1052.001 | Exfiltration Over USB | DLP, EDR |
| Exfiltration | T1567 | Exfiltration Over Web Service | CASB, Proxy |
| Exfiltration | T1048 | Exfiltration Over Alternative Protocol | Network, DLP |
| Defense Evasion | T1070 | Indicator Removal | EDR, SIEM |
| Defense Evasion | T1562 | Impair Defenses | EDR |
| Impact | T1485 | Data Destruction | EDR, SIEM |
| Impact | T1489 | Service Stop | EDR, Windows Event Log |

---

## Automation Opportunities

| Step | Automation | Tool |
|------|-----------|------|
| Behavioral anomaly detection | ML baseline + deviation alerting | Microsoft Sentinel UEBA, Varonis, Exabeam |
| Offboarding access removal | Auto-disable account on HR termination trigger | Azure AD + HR system integration |
| USB detection | Auto-alert on mass USB file copy | Symantec DLP, Microsoft Purview |
| Personal cloud upload detection | Auto-block personal cloud storage domains | CASB — Netskope, Defender for Cloud Apps |
| Evidence preservation | Auto-archive enhanced logs to WORM storage on alert | Azure Immutable Blob, AWS S3 Object Lock |
| Decoy file monitoring | Alert when honeypot/canary document is accessed | Canary Tools, OpenCanary |

---

*Back to [Main Playbook](../INCIDENT_RESPONSE_PLAYBOOK.md)*
