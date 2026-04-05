# 🌐 SolarWinds Sunburst Attack — Deep Dive Analysis

> **A comprehensive cybersecurity case study on the 2020 SolarWinds supply chain attack (SUNBURST/Solorigate), mapped to MITRE ATT&CK and the Cyber Kill Chain.**

---

## 📖 Introduction

The **SolarWinds Sunburst attack** stands as a watershed moment in cybersecurity history, fundamentally reshaping how organizations approach supply chain security and trusted software relationships. Discovered in **December 2020**, this sophisticated cyber espionage campaign demonstrated the catastrophic potential of supply chain compromises when executed by nation-state actors.

Unlike traditional cyberattacks that directly target victim organizations, this attack exploited trust relationships between a vendor and its customers — a single compromise giving attackers indirect access to **18,000+ organizations worldwide**.

> 📄 **Full Technical Report:** [`SolarWinds_Sunburst_Analysis.docx`](./SolarWinds_Sunburst_Analysis.docx)

---

## 📊 Attack at a Glance

| Field | Details |
|---|---|
| **Attack Name** | SUNBURST (a.k.a. Solorigate) |
| **Target** | SolarWinds Orion Platform |
| **Attack Vector** | Supply Chain Compromise |
| **Malware Type** | Backdoor Trojan (DLL Injection) |
| **Threat Actor** | APT29 / Cozy Bear (Russian SVR) |
| **Discovery Date** | December 2020 |
| **Stealth Duration** | ~8–9 months undetected |
| **Estimated Victims** | ~18,000 orgs received trojanized updates; ~100 actively exploited |
| **Primary Objective** | Cyber Espionage & Intelligence Gathering |
| **Sophistication** | Extremely High (Nation-State APT) |

---

## ⏱️ Attack Timeline

```
Sept 2019   ──► Initial reconnaissance & planning
Oct  2019   ──► SolarWinds network first compromised
Feb  2020   ──► SUNBURST backdoor developed & tested
Mar  2020   ──► Trojanized Orion updates first distributed
Mar–Jun 2020 ──► Widespread distribution to ~18,000 customers
Jun  2020   ──► Peak deployment; selective activation of backdoors
Dec 8 2020  ──► FireEye discovers breach, links to SolarWinds
Dec 13 2020 ──► SolarWinds publicly confirms compromise
Dec 31 2020 ──► Kill switch deployed; C2 domains sinkholed
Apr  2021   ──► U.S. sanctions imposed on Russian entities
```

### Attack Phases

| Phase | Period | Activity |
|---|---|---|
| **1. Initial Compromise** | Oct 2019 – Feb 2020 | Gained access to build environment; reconnaissance |
| **2. Development & Testing** | Feb – Mar 2020 | Built & tested the SUNBURST backdoor |
| **3. Deployment** | Mar – Jun 2020 | Injected code into Orion DLL; digitally signed |
| **4. Exploitation** | Jun – Dec 2020 | Lateral movement; data exfiltration; selective activation |
| **5. Discovery & Response** | Dec 2020 onwards | Public disclosure; forensics; remediation |

---

## 🗺️ MITRE ATT&CK Mapping

### Tactic Coverage

```
Tactic                 Coverage
──────────────────────────────────────────────
Reconnaissance         [████████░░]  80%
Resource Development   [██████████] 100%
Initial Access         [██████████] 100%
Execution              [████████░░]  80%
Persistence            [███████░░░]  70%
Privilege Escalation   [██████░░░░]  60%
Defense Evasion        [██████████] 100%
Credential Access      [█████████░]  90%
Discovery              [██████████] 100%
Lateral Movement       [████████░░]  80%
Collection             [████████░░]  80%
Command & Control      [██████████] 100%
Exfiltration           [█████████░]  90%
Impact                 [███░░░░░░░]  30%
──────────────────────────────────────────────
```

### Key Techniques Used

| Tactic | Technique ID | Technique Name |
|---|---|---|
| Initial Access | T1195.002 | Supply Chain Compromise: Software Supply Chain |
| Execution | T1053.005 | Scheduled Task/Job |
| Execution | T1129 | Shared Modules (DLL loading) |
| Persistence | T1554 | Compromise Client Software Binary |
| Defense Evasion | T1027 | Obfuscated Files or Information |
| Defense Evasion | T1036.005 | Masquerading: Match Legitimate Name |
| Defense Evasion | T1553.002 | Subvert Trust Controls: Code Signing |
| Credential Access | T1555 | Credentials from Password Stores |
| Discovery | T1082 | System Information Discovery |
| Lateral Movement | T1021.001 | Remote Services: RDP |
| C2 | T1568.002 | Dynamic Resolution: DGA |
| Exfiltration | T1041 | Exfiltration Over C2 Channel |

---

## ⚔️ Cyber Kill Chain Analysis

| Phase | Sunburst Implementation | Key Indicators |
|---|---|---|
| **1. Reconnaissance** | Mapped SolarWinds customer base; analyzed Orion architecture | Network scanning, OSINT |
| **2. Weaponization** | Developed SUNBURST backdoor with DGA; obfuscation techniques | Custom malware, evasion testing |
| **3. Delivery** | Compromised build system; injected into Orion DLL; signed with legit cert | `SolarWinds.Orion.Core.BusinessLayer.dll` |
| **4. Exploitation** | Auto-executed via legitimate Orion services; 12–14 day dormancy period | DLL side-loading, process injection |
| **5. Installation** | Persistent backdoor in trusted software; registry & scheduled tasks | Modified registry, persistent services |
| **6. C2** | DNS-based C2 via `avsvmcloud[.]com`; HTTPS + DGA | DNS beaconing, encrypted traffic |
| **7. Actions on Objectives** | Credential harvesting; lateral movement; data exfiltration; TEARDROP/RAINDROP | Email access, privilege escalation |

---

## 🛡️ Defensive Mitigations

### Breaking the Kill Chain

| Attack Phase | Primary Defenses | Detection Methods |
|---|---|---|
| Reconnaissance | Minimize attack surface; honeypots | IDS/IPS alerts, anomaly detection |
| Weaponization | Threat intelligence feeds; supply chain vetting | IOC sharing, sandbox analysis |
| Delivery | Email security; hash validation; SBOM | File integrity monitoring, web proxy logs |
| Exploitation | Patch management; least privilege; application hardening | EDR alerts, SIEM correlation |
| Installation | Application whitelisting; File Integrity Monitoring | EDR telemetry, file creation alerts |
| C2 | DNS filtering; firewall egress rules; TLS inspection | NetFlow analysis, DNS logs |
| Exfiltration | DLP solutions; privileged access management | UEBA anomalies, audit logs |

### Top MITRE Mitigations

| Mitigation ID | Strategy | Description |
|---|---|---|
| M1051 | Update Software | Rigorous patch management + software integrity verification |
| M1045 | Code Signing | Verify digital signatures; implement certificate pinning |
| M1030 | Network Segmentation | Zero Trust Architecture; micro-segmentation |
| M1027 | Password Policies | MFA enforcement; credential vaulting |
| M1031 | Network Intrusion Prevention | DNS monitoring; TLS inspection; anomaly detection |
| M1057 | Data Loss Prevention | DLP solutions; encryption requirements |

---

## 📁 Repository Contents

```
solarwinds-sunburst/
├── README.md                        ← This file
└── SolarWinds_Sunburst_Analysis.docx ← Full technical report
```

---

## 📚 Key Takeaways

1. **Supply chain attacks are highly efficient** — compromise one trusted vendor to reach thousands of downstream targets.
2. **Standard security controls failed** — digitally signed malware bypassed antivirus, whitelisting, and signature verification.
3. **Detection lag is critical** — 8-9 months undetected highlights the need for behavioral analytics over signature-based detection.
4. **Zero Trust is essential** — perimeter-based security models are insufficient against sophisticated APTs.
5. **SBOM and vendor risk management** are now baseline requirements for any enterprise security posture.

---

## 🔗 References

- [CISA Alert AA20-352A](https://www.cisa.gov/news-events/alerts/2020/12/17/advanced-persistent-threat-compromise-government-agencies-critical)
- [FireEye Blog: SUNBURST Backdoor](https://www.mandiant.com/resources/blog/evasive-attacker-leverages-solarwinds-supply-chain-compromises-with-sunburst-backdoor)
- [Microsoft Security Blog: Solorigate](https://www.microsoft.com/en-us/security/blog/2020/12/18/analyzing-solorigate-the-compromised-dll-file-that-started-a-sophisticated-cyberattack/)
- [MITRE ATT&CK: APT29](https://attack.mitre.org/groups/G0016/)

---

> **Disclaimer:** This repository is for educational and research purposes only. All analysis is based on publicly available information.
