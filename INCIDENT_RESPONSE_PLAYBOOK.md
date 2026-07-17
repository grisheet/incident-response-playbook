# Incident Response Playbook
## Cybersecurity Team — SOC-Ready Reference Guide

**Version:** 1.0  
**Last Updated:** July 2026  
**Framework Alignment:** NIST SP 800-61 Rev. 2 | MITRE ATT&CK v14  
**Classification:** Internal Use / Restricted

---

## Table of Contents

1. [Purpose & Scope](#purpose--scope)
2. [Severity Classification System](#severity-classification-system)
3. [NIST Incident Response Lifecycle](#nist-incident-response-lifecycle)
4. [Scenario 1 — Phishing Attack](playbooks/01-phishing-attack.md)
5. [Scenario 2 — Ransomware Infection](playbooks/02-ransomware-infection.md)
6. [Scenario 3 — Data Breach](playbooks/03-data-breach.md)
7. [Scenario 4 — Insider Threat](playbooks/04-insider-threat.md)
8. [Automation Opportunities](#automation-opportunities)
9. [Roles & Responsibilities](#roles--responsibilities)
10. [Communication Templates](#communication-templates)
11. [Tool Reference](#tool-reference)

---

## Purpose & Scope

This playbook provides structured, actionable guidance for cybersecurity incident response teams. It is designed to be used by Security Operations Center (SOC) analysts, Incident Response (IR) leads, and IT Security personnel.

### Covered Threat Scenarios
| # | Scenario | Severity Range | Primary MITRE Tactic |
|---|----------|---------------|----------------------|
| 1 | Phishing Attack | P3 – P1 | Initial Access (TA0001) |
| 2 | Ransomware Infection | P2 – P1 | Impact (TA0040) |
| 3 | Data Breach | P2 – P1 | Exfiltration (TA0010) |
| 4 | Insider Threat | P3 – P1 | Collection (TA0009) |

### Out of Scope
- Physical security incidents
- Non-cybersecurity-related system outages
- Third-party vendor incidents (handled via Vendor IR process)

---

## Severity Classification System

| Priority | Label | Description | Response SLA | Example |
|----------|-------|-------------|-------------|---------||
| **P1** | Critical | Active breach, data exfiltration, ransomware spreading | Immediate — within 15 min | Ransomware detected on 10+ endpoints |
| **P2** | High | Confirmed compromise, limited blast radius | Within 1 hour | Single endpoint compromised, credentials stolen |
| **P3** | Medium | Suspicious activity, potential threat indicators | Within 4 hours | Phishing email reported, no confirmed click |
| **P4** | Low | Informational, policy violation, low risk | Within 24 hours | User visiting blocked site |

### Severity Escalation Triggers
- **Escalate to P1** if: data exfiltration confirmed, >5 systems affected, executive credentials involved, ransomware spreading laterally
- **Escalate to P2** if: confirmed malware execution, privileged account compromise, sensitive data accessed
- **Downgrade** only after full eradication and verification

---

## NIST Incident Response Lifecycle

All scenarios in this playbook follow the **NIST SP 800-61 Rev. 2** four-phase lifecycle:

```
+------------------+     +------------------+     +------------------+     +------------------+
|   PREPARATION    | --> |  DETECTION &     | --> |  CONTAINMENT,    | --> |   POST-INCIDENT  |
|                  |     |  ANALYSIS        |     |  ERADICATION &   |     |   ACTIVITY       |
|  - Policies      |     |  - Monitoring    |     |  RECOVERY        |     |  - Lessons       |
|  - Training      |     |  - Triage        |     |  - Isolate       |     |    Learned       |
|  - Tools         |     |  - Investigation |     |  - Remove        |     |  - Reporting     |
|  - Playbooks     |     |  - Escalation    |     |  - Restore       |     |  - Improvements  |
+------------------+     +------------------+     +------------------+     +------------------+
```

### Phase Definitions

#### Phase 1 — Preparation
- Maintain up-to-date IR playbooks
- Ensure SIEM (e.g., Splunk, Microsoft Sentinel) is tuned
- Conduct tabletop exercises quarterly
- Establish communication trees and escalation contacts
- Deploy EDR (e.g., CrowdStrike Falcon, Microsoft Defender for Endpoint)

#### Phase 2 — Detection & Analysis
- Monitor SIEM alerts and EDR telemetry
- Correlate IOCs (Indicators of Compromise)
- Assign severity using classification table above
- Open incident ticket and assign IR lead
- Preserve evidence (memory dumps, logs, disk images)

#### Phase 3 — Containment, Eradication & Recovery
- Isolate affected systems immediately
- Block malicious IPs/domains at firewall/proxy
- Remove malware / revoke compromised credentials
- Patch exploited vulnerabilities
- Restore from clean backups
- Validate systems before returning to production

#### Phase 4 — Post-Incident Activity
- Conduct post-mortem within 5 business days
- Document timeline, root cause, and impact
- Update playbooks and detection rules
- Report to leadership / regulators as required
- Track remediation items to closure

---

## Automation Opportunities

| Scenario | Automation Use Case | Tool Examples |
|----------|--------------------|--------------|
| Phishing | Auto-quarantine emails matching IOC signatures | Microsoft Defender, Proofpoint |
| Phishing | Automated URL detonation and classification | Any.run, VirusTotal API |
| Ransomware | Auto-isolate endpoint on ransomware behavioral detection | CrowdStrike Falcon, Cortex XSOAR |
| Ransomware | Automated snapshot/backup verification | AWS Backup, Azure Recovery |
| Data Breach | DLP alert auto-escalation to P1 on bulk exfiltration | Microsoft Purview, Forcepoint |
| Data Breach | Automated regulatory breach notification draft | ServiceNow, Splunk SOAR |
| Insider Threat | UEBA anomaly auto-ticket creation | Microsoft Sentinel, Varonis |
| All | IOC enrichment via threat intelligence feeds | MISP, VirusTotal, Recorded Future |
| All | Automated evidence collection and preservation | Velociraptor, GRR Rapid Response |

---

## Roles & Responsibilities

| Role | Responsibilities |
|------|----------------|
| **IR Lead** | Coordinate overall response, make containment decisions, communicate with leadership |
| **SOC Analyst (L1)** | Initial alert triage, severity assignment, escalation |
| **SOC Analyst (L2/L3)** | Deep investigation, malware analysis, forensics |
| **IT Operations** | System isolation, backup restoration, network changes |
| **Legal / Compliance** | Regulatory notification, evidence chain of custody |
| **Communications / PR** | External communications if breach is public |
| **CISO** | P1 escalation approval, executive reporting |

---

## Communication Templates

### Internal Escalation (P1/P2)
```
SUBJECT: [P1 INCIDENT] <Incident Type> - <Short Description>

Incident ID: INC-YYYY-XXXX
Severity: P1 / Critical
Detected: <Date/Time UTC>
Affected Systems: <List>
Current Status: <Containment in progress / Investigating>
IR Lead: <Name>
Next Update: In 30 minutes

Summary: <2-3 sentence description>
```

### Executive Summary Template
```
INCIDENT EXECUTIVE SUMMARY
Date: | Incident ID: | Severity:
What Happened: 
Impact: 
Actions Taken: 
Current Status: 
Next Steps: 
Estimated Resolution: 
```

---

## Tool Reference

| Category | Tool | Use Case |
|----------|------|----------|
| SIEM | Splunk, Microsoft Sentinel, IBM QRadar | Log aggregation, alert correlation |
| EDR | CrowdStrike Falcon, Microsoft Defender for Endpoint, SentinelOne | Endpoint detection, isolation |
| SOAR | Cortex XSOAR, Splunk SOAR, Microsoft Sentinel Playbooks | Response automation |
| Threat Intel | MISP, VirusTotal, Recorded Future, Shodan | IOC enrichment |
| Forensics | Velociraptor, FTK, Autopsy, Volatility | Evidence collection & analysis |
| Network | Wireshark, Zeek, Darktrace | Network traffic analysis |
| Vulnerability | Tenable Nessus, Qualys, Rapid7 InsightVM | Vulnerability identification |
| Email Security | Proofpoint, Mimecast, Microsoft Defender for Office 365 | Email threat analysis |
| DLP | Microsoft Purview, Forcepoint, Symantec DLP | Data loss prevention |
| Identity | CyberArk, BeyondTrust, Azure AD PIM | Privileged access management |

---

*This playbook is a living document. Review and update quarterly or after every P1/P2 incident.*

**Document Owner:** CISO / Security Operations  
**Next Review Date:** October 2026
