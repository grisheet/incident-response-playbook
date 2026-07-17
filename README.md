# Incident Response Playbook for Cybersecurity Teams

> A professional, SOC-ready Incident Response Playbook aligned with **NIST SP 800-61 Rev. 2** and **MITRE ATT&CK v14**, covering the four most critical cybersecurity threat scenarios.

---

## Overview

This repository contains a complete, production-ready Incident Response (IR) Playbook designed for Security Operations Center (SOC) teams, IR analysts, and cybersecurity professionals. Each playbook scenario follows the NIST Incident Response Lifecycle and includes MITRE ATT&CK technique mappings, real-world tooling examples, automation opportunities, and actionable checklists.

### Why This Playbook?
- **Structured & Actionable** — Bullet-based checklists usable in live incidents
- **Framework-Aligned** — NIST SP 800-61 Rev. 2 + MITRE ATT&CK v14
- **Tool-Referenced** — Specific SIEM, EDR, SOAR, and DLP tool examples throughout
- **Regulatory Aware** — GDPR, HIPAA, PCI DSS, SEC, CCPA notification requirements
- **Automation-Ready** — Automation opportunities identified for each scenario

---

## Repository Structure

```
incident-response-playbook/
├── INCIDENT_RESPONSE_PLAYBOOK.md     # Master playbook: scope, severity system, NIST lifecycle,
│                                     # roles, communication templates, tool reference
├── playbooks/
│   ├── 01-phishing-attack.md           # Phishing attack response (P3-P1)
│   ├── 02-ransomware-infection.md      # Ransomware response (P2-P1, critical)
│   ├── 03-data-breach.md               # Data breach response (P2-P1, regulatory)
│   └── 04-insider-threat.md            # Insider threat response (P3-P1, HR/Legal)
└── README.md
```

---

## Scenarios Covered

### 1. Phishing Attack
**File:** [`playbooks/01-phishing-attack.md`](playbooks/01-phishing-attack.md)  
**Severity:** P3 – P1 | **Primary Tactic:** Initial Access (TA0001)

Covers detection of spearphishing emails and credential harvesting pages, triage from "reported, no click" through to "credentials stolen, lateral movement confirmed," email quarantine procedures, endpoint isolation, OAuth token revocation, and post-incident phishing simulation steps.

**Key Tools:** Proofpoint, Microsoft Defender for O365, CrowdStrike Falcon, Azure AD, VirusTotal API

---

### 2. Ransomware Infection
**File:** [`playbooks/02-ransomware-infection.md`](playbooks/02-ransomware-infection.md)  
**Severity:** P2 – P1 (treated as P1 by default) | **Primary Tactic:** Impact (TA0040)

Covers behavioral detection of mass file encryption, shadow copy deletion, lateral movement containment, ransom decision framework (including OFAC sanctions check), ordered recovery sequence (Domain Controllers → infrastructure → endpoints), and regulatory notification requirements.

**Key Tools:** CrowdStrike Falcon, Cortex XSOAR, Velociraptor, ID Ransomware, FBI IC3

---

### 3. Data Breach
**File:** [`playbooks/03-data-breach.md`](playbooks/03-data-breach.md)  
**Severity:** P2 – P1 | **Primary Tactic:** Exfiltration (TA0010)

Covers DLP and CASB detection of bulk data exfiltration, dark web monitoring triggers, stopping active exfiltration, cloud storage bucket lockdown, and a complete regulatory notification requirements table (GDPR 72hr, HIPAA 60-day, PCI DSS immediate, SEC 4-day).

**Key Tools:** Microsoft Purview DLP, Netskope CASB, Recorded Future, AWS CloudTrail, Forcepoint

---

### 4. Insider Threat
**File:** [`playbooks/04-insider-threat.md`](playbooks/04-insider-threat.md)  
**Severity:** P3 – P1 | **Primary Tactic:** Collection (TA0009)

Covers all four insider threat categories (Malicious, Negligent, Compromised, Colluding), UEBA behavioral detection, covert investigation phase (monitoring without alerting subject), HR/Legal coordination requirements, chain of custody evidence handling, and civil/criminal referral procedures.

**Key Tools:** Microsoft Sentinel UEBA, Varonis, Exabeam, Canary Tools, Microsoft Purview

---

## Severity Classification

| Priority | Label | Response SLA | Description |
|----------|-------|-------------|-------------|
| **P1** | Critical | 15 minutes | Active breach, spreading ransomware, confirmed exfiltration |
| **P2** | High | 1 hour | Confirmed compromise, limited scope |
| **P3** | Medium | 4 hours | Suspicious activity, potential threat indicators |
| **P4** | Low | 24 hours | Policy violation, informational |

---

## NIST Incident Response Lifecycle

All playbooks follow the four-phase NIST SP 800-61 Rev. 2 lifecycle:

```
Preparation --> Detection & Analysis --> Containment/Eradication/Recovery --> Post-Incident
```

Each scenario includes phase-specific steps with time-boxed SLAs.

---

## Framework Alignment

| Framework | Version | Coverage |
|-----------|---------|----------|
| NIST SP 800-61 | Rev. 2 | Full lifecycle (all 4 phases) |
| MITRE ATT&CK | v14 | Technique-level mapping for all 4 scenarios |
| GDPR | Current | 72-hour notification requirement |
| HIPAA | Current | Breach notification rule |
| PCI DSS | v4.0 | Incident response requirement |
| NIST CSF | 2.0 | Respond and Recover functions |

---

## Tool Reference (Quick View)

| Category | Examples |
|----------|----------|
| SIEM | Splunk, Microsoft Sentinel, IBM QRadar |
| EDR | CrowdStrike Falcon, Microsoft Defender for Endpoint, SentinelOne |
| SOAR | Cortex XSOAR, Splunk SOAR, Microsoft Sentinel Playbooks |
| Email Security | Proofpoint, Mimecast, Microsoft Defender for Office 365 |
| DLP | Microsoft Purview, Forcepoint, Symantec DLP |
| CASB | Microsoft Defender for Cloud Apps, Netskope |
| UEBA | Microsoft Sentinel, Varonis, Exabeam |
| Threat Intel | MISP, VirusTotal, Recorded Future |
| Forensics | Velociraptor, Autopsy, Volatility, FTK |
| Identity | CyberArk, BeyondTrust, Azure AD PIM |

---

## How to Use This Playbook

1. **During an active incident:** Open the relevant scenario playbook and work through the checklists in order
2. **For tabletop exercises:** Use each scenario as a discussion guide
3. **For SOC onboarding:** Use the master playbook + tool reference for new analyst training
4. **For compliance documentation:** Reference NIST alignment and regulatory notification tables
5. **For automation development:** Use the Automation Opportunities tables in each scenario

---

## Maintenance

- Review playbooks **quarterly** or after every P1/P2 incident
- Update MITRE ATT&CK mappings when new major versions release
- Validate tool references remain current with your organization's stack
- Add new scenarios as threat landscape evolves

---

## Contributing

To contribute new scenarios or improvements:
1. Fork the repository
2. Create a feature branch: `git checkout -b scenario/ddos-attack`
3. Follow the existing playbook format (Detection → Triage → Containment → Eradication → Recovery → Post-Incident → MITRE Mapping → Automation)
4. Submit a pull request with a description of the scenario and key sources

---

## License

This project is licensed under the MIT License — free for use by security teams and individuals.

---

**Document Owner:** Security Operations / CISO  
**Version:** 1.0  
**Last Updated:** July 2026  
**Next Review:** October 2026
