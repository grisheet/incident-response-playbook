# Scenario 1: Phishing Attack

**Severity Range:** P3 (Reported, no click) to P1 (Credentials stolen, lateral movement)  
**MITRE ATT&CK Tactics:** Initial Access (TA0001), Credential Access (TA0006), Execution (TA0002)  
**Primary MITRE Techniques:**
- T1566.001 — Spearphishing Attachment
- T1566.002 — Spearphishing Link
- T1078 — Valid Accounts (post-compromise)
- T1059 — Command and Scripting Interpreter

---

## Detection Methods

### Automated Detection
- [ ] SIEM alert: Multiple failed login attempts from new geolocation after email open event
- [ ] Email gateway (Proofpoint / Mimecast / Defender for O365) flags URL as malicious
- [ ] EDR detects suspicious process spawned by Outlook (e.g., `cmd.exe` child of `outlook.exe`)
- [ ] Anti-phishing policy triggers: spoofed sender domain, lookalike domains
- [ ] SIEM correlation: Email open + URL click + new external IP login within 10 minutes
- [ ] DLP alert: credential data exfiltration from compromised account

### User-Reported Detection
- [ ] User submits suspicious email via Phish Alert Button (KnowBe4 / Cofense)
- [ ] Help desk ticket: "I think I clicked a phishing link"
- [ ] Manager reports employee received suspicious email with company branding

### Threat Intelligence Indicators
- [ ] IOC match in threat intel feed: known phishing domain/IP/URL
- [ ] MISP or VirusTotal hit on attachment hash
- [ ] Newly registered domain (< 30 days old) in email header

---

## Triage Steps

**Assign Severity:**
- P4: Suspicious email reported, no user interaction
- P3: User clicked link, no credential entry confirmed
- P2: Credentials entered on phishing page, single account
- P1: Multiple accounts compromised, lateral movement detected

**Initial Questions (SOC L1):**
- [ ] Did the user click the link or open the attachment?
- [ ] Did the user enter credentials on any page?
- [ ] Has the account shown any unusual sign-in activity?
- [ ] How many users received this email? (Check email gateway logs)
- [ ] Is the sender internal or external?

**Immediate Evidence Collection:**
- [ ] Obtain full email headers (`.eml` file)
- [ ] Extract URLs from email body — do NOT click; use URLScan.io or Any.run sandbox
- [ ] Capture attachment hash (SHA256) — submit to VirusTotal
- [ ] Pull user sign-in logs from Azure AD / Okta / Duo
- [ ] Capture EDR telemetry from user endpoint (CrowdStrike Falcon / Defender)
- [ ] Screenshot or export SIEM alert timeline

---

## Containment Strategy

### Short-Term Containment (0–30 minutes)
- [ ] **Quarantine phishing email** from all mailboxes: `Search-Mailbox` (Exchange) or Compliance Center Purge
- [ ] **Block malicious URL/domain** at email gateway, proxy, and DNS (Cisco Umbrella / Zscaler)
- [ ] **Disable compromised account(s)** in Azure AD / Active Directory immediately
- [ ] **Revoke active sessions** and OAuth tokens for affected accounts (Azure AD: Revoke Sign-in Sessions)
- [ ] **Force MFA re-enrollment** if MFA was bypassed
- [ ] **Isolate endpoint** if EDR detects payload execution (CrowdStrike Network Contain)
- [ ] **Block IOC hash** in EDR policy and AV signatures

### Long-Term Containment (30 min – 4 hours)
- [ ] Hunt for similar emails across all mailboxes (sender pattern, subject, URL pattern)
- [ ] Search SIEM for other users who clicked the URL
- [ ] Check for OAuth app consent grants from compromised accounts
- [ ] Review email forwarding rules created by compromised account
- [ ] Alert HR/Legal if PII or financial data was accessed

---

## Eradication Steps

- [ ] Delete all instances of phishing email from all mailboxes (use Compliance Center Content Search)
- [ ] Remove any malicious inbox rules or auto-forwarding rules created by attacker
- [ ] Revoke and rotate compromised credentials — force password reset
- [ ] Remove attacker-registered OAuth app consents (Azure AD Enterprise Apps)
- [ ] Remove any persistence mechanisms detected by EDR (scheduled tasks, registry run keys)
- [ ] Re-image endpoint if malware payload was executed (confirmed by EDR)
- [ ] Verify no backdoor accounts or admin role escalations occurred
- [ ] Block attacker IP ranges at perimeter firewall

---

## Recovery Procedures

- [ ] Re-enable account(s) after password reset and MFA verification
- [ ] Return endpoint to production after EDR clean bill of health
- [ ] Monitor account for 72 hours post-recovery for anomalous activity
- [ ] Verify email forwarding rules and mailbox permissions are clean
- [ ] Re-validate MFA enrollment for affected user(s)
- [ ] Confirm DLP policies blocked any data exfiltration attempts
- [ ] Notify affected user and their manager of resolution
- [ ] Update email gateway block lists with new phishing IOCs

---

## Post-Incident Actions

- [ ] **Document full timeline:** first email delivery → detection → containment → resolution
- [ ] **Root cause analysis:** How did email bypass gateway filters? Why was user susceptible?
- [ ] **Lessons learned meeting** within 5 business days
- [ ] **Phishing simulation** targeting affected department within 30 days
- [ ] **Security awareness training** assigned to user who clicked
- [ ] **Update email gateway rules:** new sender patterns, URL patterns
- [ ] **Submit IOCs to threat sharing platforms** (MISP, ISAC)
- [ ] **File regulatory notification** if PII was exfiltrated (GDPR 72hr, HIPAA 60-day)
- [ ] **Update this playbook** with new TTPs observed

---

## MITRE ATT&CK Mapping

| Phase | Technique ID | Technique Name | Detection Source |
|-------|-------------|----------------|------------------|
| Initial Access | T1566.001 | Spearphishing Attachment | Email Gateway, EDR |
| Initial Access | T1566.002 | Spearphishing Link | Email Gateway, Proxy |
| Execution | T1059.001 | PowerShell | EDR, Windows Event Log |
| Credential Access | T1056.001 | Keylogging | EDR |
| Credential Access | T1539 | Steal Web Session Cookie | Proxy, SIEM |
| Persistence | T1137 | Office Application Startup | EDR |
| Persistence | T1098 | Account Manipulation | Azure AD Logs |
| Lateral Movement | T1078 | Valid Accounts | SIEM, Azure AD |
| Exfiltration | T1048 | Exfiltration Over Alternative Protocol | DLP, Network |

---

## Automation Opportunities

| Step | Automation | Tool |
|------|-----------|------|
| Email quarantine | Auto-quarantine on gateway IOC match | Proofpoint TRAP, Defender |
| URL detonation | Sandbox URL on click detection | Any.run, Defender Safe Links |
| Account disable | Auto-disable account on P1 alert | Azure Logic Apps, Cortex XSOAR |
| IOC enrichment | Auto-enrich sender/URL in ticket | VirusTotal API, MISP |
| Session revocation | Auto-revoke sessions on confirmed phish | Microsoft Sentinel Playbook |
| Threat hunt | Scheduled hunt for similar email patterns | KQL query, Defender Advanced Hunting |

---

*Back to [Main Playbook](../INCIDENT_RESPONSE_PLAYBOOK.md)*
