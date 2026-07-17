# Scenario 2: Ransomware Infection

**Severity Range:** P2 (Isolated endpoint) to P1 (Lateral spread, backup corruption)  
**MITRE ATT&CK Tactics:** Execution (TA0002), Persistence (TA0003), Lateral Movement (TA0008), Impact (TA0040)  
**Primary MITRE Techniques:**
- T1486 — Data Encrypted for Impact
- T1490 — Inhibit System Recovery
- T1021 — Remote Services (lateral movement)
- T1562.001 — Disable or Modify Tools (AV/EDR tampering)
- T1078 — Valid Accounts

> **CRITICAL NOTE:** Ransomware incidents are treated as P1 by default until scope is confirmed. Speed of containment directly limits blast radius. Do NOT pay ransom without CISO and Legal approval.

---

## Detection Methods

### Automated Detection
- [ ] EDR behavioral alert: Mass file encryption activity (CrowdStrike, Defender for Endpoint)
- [ ] SIEM alert: Sudden spike in file rename/overwrite events (`.locked`, `.encrypted`, `.STOP` extensions)
- [ ] EDR alert: Shadow copy deletion (`vssadmin delete shadows`, `wmic shadowcopy delete`)
- [ ] EDR alert: Disabling of Windows Defender / stopping security services
- [ ] SIEM alert: Lateral movement via SMB, RDP, PsExec across multiple hosts
- [ ] Network alert: Anomalous outbound traffic to C2 infrastructure (pre-encryption beaconing)
- [ ] Backup solution alert: Backup files being deleted or encrypted
- [ ] HoneyFile trigger: Encrypted canary file in monitored directory

### User-Reported Detection
- [ ] User reports files are inaccessible or have unknown extensions
- [ ] User sees ransom note (README.txt, DECRYPT_FILES.html) on desktop
- [ ] User reports system dramatically slowed or unresponsive
- [ ] IT reports network share files are corrupted or renamed

---

## Triage Steps

**Assign Severity:**
- P2: Single endpoint affected, network isolated, backups intact
- P1: Multiple endpoints affected OR backup corruption detected OR lateral movement in progress

**Immediate Triage Questions (SOC L1 — first 10 minutes):**
- [ ] How many systems are showing encryption activity?
- [ ] Is the infection still spreading (active lateral movement)?
- [ ] Are network shares affected?
- [ ] Are backups (on-prem and cloud) still accessible and unmodified?
- [ ] What was the initial access vector? (phishing? RDP brute force? Exploited vulnerability?)
- [ ] What ransomware family/variant? (check ransom note, file extension, VirusTotal)
- [ ] Is the system domain-joined? What's the blast radius risk?

**Immediate Evidence Collection (before isolation if safe to do so):**
- [ ] Capture memory dump of affected system (WinPmem, Magnet RAM Capture)
- [ ] Collect EDR forensic artifacts and telemetry
- [ ] Document exact time of first encryption event (EDR logs, Windows Event Log)
- [ ] Capture ransom note content and screenshot
- [ ] Identify Patient Zero (first infected system)
- [ ] Pull network flow logs to identify C2 communication
- [ ] Collect list of all encrypted file extensions and affected directories

---

## Containment Strategy

### IMMEDIATE Containment (0–15 minutes — P1 Priority)

> Stop the bleeding first. Containment before investigation.

- [ ] **Network isolate ALL affected endpoints** via EDR (CrowdStrike Network Contain / Defender Isolate)
- [ ] **Disable compromised accounts** identified in lateral movement path
- [ ] **Block C2 IP addresses and domains** at perimeter firewall and DNS sinkhole
- [ ] **Shut down vulnerable network services:** RDP (3389), SMB (445) if not business-critical
- [ ] **Segment affected VLANs** at network switch level to prevent further spread
- [ ] **Take cloud VM snapshots** before any remediation (preserve forensic state)
- [ ] **Notify CISO and IR Lead** immediately — P1 war room activated
- [ ] **Do NOT reboot infected systems** — may destroy memory-resident malware evidence

### Short-Term Containment (15 min – 2 hours)
- [ ] Conduct active threat hunt for ransomware beaconing on all endpoints (EDR query)
- [ ] Identify and quarantine additional infected hosts discovered in hunt
- [ ] Determine if Active Directory / domain controllers are compromised
- [ ] Assess backup integrity — verify last known good backup timestamp
- [ ] Establish secure out-of-band communication channel for IR team
- [ ] Preserve all affected systems in isolated network segment for forensics

### Long-Term Containment (2–24 hours)
- [ ] Identify initial access vector and close it (patch, disable service, reset credentials)
- [ ] Audit all privileged accounts for compromise
- [ ] Verify no persistence mechanisms remain on non-encrypted systems
- [ ] Assess total scope of encrypted data (business impact assessment)

---

## Eradication Steps

- [ ] Identify and document complete malware infection chain (dropper → payload → ransomware)
- [ ] Remove malware artifacts: executables, scripts, scheduled tasks, registry run keys
- [ ] Reset ALL passwords for domain accounts (prioritize admins, service accounts)
- [ ] Rotate all service account credentials and API keys
- [ ] Revoke and reissue certificates if PKI infrastructure was accessed
- [ ] Re-image all confirmed ransomware-infected endpoints (do not restore from potentially infected backups)
- [ ] Patch the exploited vulnerability (CVE) that enabled initial access
- [ ] Remove attacker persistence: malicious GPOs, scheduled tasks, WMI subscriptions
- [ ] Confirm no backdoor/RAT remains on any system in the environment
- [ ] Update EDR/AV signatures with new ransomware IOCs

---

## Recovery Procedures

### Restore Sequence (critical order)
1. [ ] Verify clean, pre-infection backups exist and are accessible
2. [ ] Restore Domain Controllers first (if affected)
3. [ ] Restore critical infrastructure servers (file servers, database servers)
4. [ ] Restore business-critical applications
5. [ ] Restore user endpoints last
6. [ ] Validate data integrity after each restore before going live

### Recovery Validation Checklist
- [ ] Restored systems show no active malware processes (EDR clean scan)
- [ ] Network traffic from restored systems appears normal (no C2 beaconing)
- [ ] File encryption activity has fully ceased
- [ ] Backups and shadow copies are intact on restored systems
- [ ] All accounts have new credentials
- [ ] Security tools (AV, EDR) are functioning and up-to-date
- [ ] Business applications are functional and data verified as uncorrupted
- [ ] Systems monitored for 48–72 hours before declaring full recovery

### Ransom Decision Framework
- [ ] Engage Legal and CISO before any ransom consideration
- [ ] Check OFAC sanctions list (paying certain groups may be illegal)
- [ ] Engage cyber insurance provider
- [ ] Consult FBI/CISA (they may have decryptors available)
- [ ] Payment does NOT guarantee decryption — recover from backups where possible

---

## Post-Incident Actions

- [ ] **Executive briefing** within 2 hours of containment (P1 requirement)
- [ ] **Regulatory notification:** Check GDPR (72hr), HIPAA, PCI DSS, SEC requirements
- [ ] **Law enforcement notification:** File report with FBI IC3, CISA
- [ ] **Full post-mortem** within 5 business days: timeline, root cause, business impact ($)
- [ ] **Lessons learned:** How did ransomware bypass defenses? Which controls failed?
- [ ] **Architecture review:** Air-gapped backups, network segmentation improvements, EDR coverage gaps
- [ ] **Tabletop exercise** within 60 days simulating this specific attack chain
- [ ] **Update IR playbook** with new ransomware family TTPs
- [ ] **Cyber insurance claim** documentation compiled
- [ ] **Board-level report** if material financial impact

---

## MITRE ATT&CK Mapping

| Phase | Technique ID | Technique Name | Detection Source |
|-------|-------------|----------------|------------------|
| Initial Access | T1566 | Phishing | Email Gateway |
| Initial Access | T1133 | External Remote Services | Firewall, SIEM |
| Execution | T1059 | Command and Scripting Interpreter | EDR |
| Persistence | T1547.001 | Registry Run Keys | EDR, Windows Event Log |
| Defense Evasion | T1562.001 | Disable or Modify Tools | EDR |
| Credential Access | T1003 | OS Credential Dumping | EDR |
| Lateral Movement | T1021.001 | Remote Desktop Protocol | SIEM, Network |
| Lateral Movement | T1021.002 | SMB/Windows Admin Shares | Network, EDR |
| Discovery | T1083 | File and Directory Discovery | EDR |
| Impact | T1486 | Data Encrypted for Impact | EDR, File Monitoring |
| Impact | T1490 | Inhibit System Recovery | EDR, Windows Event Log |
| Impact | T1489 | Service Stop | EDR, Windows Event Log |

---

## Automation Opportunities

| Step | Automation | Tool |
|------|-----------|------|
| Ransomware detection | Behavioral heuristic on mass file encryption | CrowdStrike Falcon, Defender |
| Endpoint isolation | Auto-isolate on ransomware signature detection | Cortex XSOAR, Sentinel Playbook |
| C2 blocking | Auto-block known ransomware C2 IPs via threat feed | Palo Alto XSOAR, Cisco Umbrella |
| Backup verification | Automated backup integrity check on alert | Azure Backup, AWS Backup |
| Account lockout | Auto-disable accounts with ransomware lateral movement | Azure AD Conditional Access |
| Evidence collection | Automated forensic artifact collection | Velociraptor, GRR |
| Ransom note parsing | Auto-identify ransomware family from note text | ID Ransomware API |

---

*Back to [Main Playbook](../INCIDENT_RESPONSE_PLAYBOOK.md)*
