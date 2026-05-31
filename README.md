# 🛡️ Home SOC Lab — Threat Detection with Splunk SIEM

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Platform](https://img.shields.io/badge/platform-VirtualBox-orange.svg)
![SIEM](https://img.shields.io/badge/SIEM-Splunk%20Enterprise%2010.2.3-green.svg)
![Sysmon](https://img.shields.io/badge/Sysmon-v15.20-blue.svg)
![MITRE](https://img.shields.io/badge/MITRE%20ATT%26CK-T1110%20%7C%20T1059%20%7C%20T1046-red.svg)
![Status](https://img.shields.io/badge/status-Active-brightgreen.svg)
![Events](https://img.shields.io/badge/events%20ingested-6%2C209%2B-orange.svg)

> A fully functional Security Operations Centre (SOC) home lab built from scratch using VirtualBox, Splunk Enterprise, and Sysmon — simulating a real enterprise environment with live log ingestion, threat detection, and MITRE ATT&CK mapped alerting.

---

## 📋 Table of Contents

- [Project Overview](#-project-overview)
- [Lab Architecture](#-lab-architecture)
- [Technologies Used](#-technologies-used)
- [Lab Metrics](#-lab-metrics)
- [Setup Phases](#-setup-phases)
- [Detection Rules](#-detection-rules)
- [MITRE ATT&CK Coverage](#-mitre-attck-coverage)
- [Project Screenshots](#-project-screenshots)
- [Challenges and Solutions](#-challenges-and-solutions)
- [Key Contributions](#-key-contributions)
- [License](#-license)

---

## 🎯 Project Overview

This project demonstrates hands-on SOC analyst skills by building a complete threat detection environment from scratch. The lab includes:

- **3 Virtual Machines** on an isolated internal network (SOC-Lab)
- **Splunk Enterprise SIEM** ingesting 6,209+ live events
- **Sysmon v15.20** collecting detailed Windows telemetry
- **4 Custom SPL Detection Rules** mapped to MITRE ATT&CK
- **Automated Alerts** running every 5 minutes
- **SOC Dashboard** in Splunk Dashboard Studio with real-time panels

---

## 🏗️ Lab Architecture

```
┌─────────────────────────────────────────────────────────┐
│              SOC-Lab Internal Network                    │
│                  192.168.10.0/24                         │
│                                                          │
│  ┌──────────────────┐      ┌──────────────────┐         │
│  │  Windows 10 Pro  │      │   Kali Linux     │         │
│  │     Victim       │      │    Attacker      │         │
│  │  192.168.10.10   │      │  192.168.10.20   │         │
│  │                  │      │                  │         │
│  │  ✅ Sysmon v15   │      │  ✅ Nmap         │         │
│  │  ✅ Splunk UF    │      │  ✅ Metasploit   │         │
│  └────────┬─────────┘      └──────────────────┘         │
│           │ logs (port 9997)                             │
│           ▼                                              │
│  ┌──────────────────┐                                   │
│  │  Ubuntu Server   │                                   │
│  │   Splunk SIEM    │                                   │
│  │  192.168.10.30   │                                   │
│  │                  │                                   │
│  │  ✅ Splunk 10.2  │                                   │
│  │  ✅ Port 8000    │                                   │
│  │  ✅ Port 9997    │                                   │
│  └──────────────────┘                                   │
└─────────────────────────────────────────────────────────┘
```

---

## 🛠️ Technologies Used

| Tool | Version | Purpose |
|------|---------|---------|
| VirtualBox | 7.0.20 | Virtualization platform |
| Windows 10 Pro | 22H2 | Target/victim machine |
| Kali Linux | 2026.1 | Attack simulation |
| Ubuntu Server | 26.04 LTS | SIEM host OS |
| Splunk Enterprise | 10.2.3 | SIEM platform |
| Splunk Universal Forwarder | 10.4.0 | Log shipping agent |
| Sysmon | v15.20 | Windows telemetry |
| SwiftOnSecurity Config | Latest | Sysmon ruleset |

---

## 📊 Lab Metrics

```
╔══════════════════════════════════════════════════════╗
║              SOC LAB — KEY METRICS                   ║
╠══════════════════════════════════════════════════════╣
║  Total Events Ingested       │  6,209+               ║
║  Sysmon Events (index=sysmon)│  2,167+               ║
║  Windows Security Events     │  4,042+               ║
║  Detection Rules Authored    │  4                    ║
║  MITRE ATT&CK Techniques     │  4 (T1110/T1059/T1046)║
║  Scheduled Alerts            │  4 (every 5 min)      ║
║  Dashboard Panels            │  3                    ║
║  VMs Built                   │  3                    ║
║  Snapshots Taken             │  9                    ║
╚══════════════════════════════════════════════════════╝
```

### Event Distribution

```
Events by Source
════════════════════════════════════════
Windows Security    ████████████████░░░░  65%  (4,042 events)
Sysmon Operational  ████████████░░░░░░░░  35%  (2,167 events)
════════════════════════════════════════

Detection Coverage by MITRE Technique
════════════════════════════════════════
T1110  Brute Force          ██████████  HIGH
T1059  Command Scripting    ██████████  HIGH
T1059.001 PowerShell        ██████████  HIGH
T1046  Network Scanning     ████████░░  MEDIUM
════════════════════════════════════════

Sysmon EventCode Breakdown
════════════════════════════════════════
EventCode 1  Process Create  ████████████████  75%
EventCode 3  Network Connect ████░░░░░░░░░░░░  10%
EventCode 13 Registry        ███░░░░░░░░░░░░░   8%
Other codes                  ███░░░░░░░░░░░░░   7%
════════════════════════════════════════
```

---

## ⚙️ Setup Phases

### Phase 1 — Lab Infrastructure

| Step | Action | Result |
|------|--------|--------|
| 1 | Installed VirtualBox 7.0.20 | Virtualization platform ready |
| 2 | Configured D:\SOC-Lab\VMs as default folder | All VMs stored on D drive |
| 3 | Downloaded 3 OS ISOs | Windows 10, Kali 2026.1, Ubuntu 26.04 |
| 4 | Created and installed 3 VMs | All OS installations complete |
| 5 | Installed Guest Additions | Smooth VM performance |
| 6 | Set Internal Network "SOC-Lab" | Isolated lab network |
| 7 | Assigned static IPs | .10, .20, .30 confirmed |
| 8 | Verified ping between all VMs | Network connectivity confirmed |
| 9 | Took snapshots | Clean-Baseline saved |

### Phase 2 — Splunk SIEM

| Step | Action | Result |
|------|--------|--------|
| 1 | Switched Ubuntu to NAT temporarily | Internet access for download |
| 2 | Fixed DNS (8.8.8.8) | Name resolution working |
| 3 | Downloaded Splunk 10.2.3 .deb | 357MB package downloaded |
| 4 | Installed Splunk | dpkg -i successful |
| 5 | Created admin account | admin / Splunk@Lab1 |
| 6 | Created sysmon + windows indexes | Indexes ready |
| 7 | Enabled port 9997 receiving | Splunk listening |
| 8 | Switched back to Internal Network | Lab isolation restored |
| 9 | Verified UI at 192.168.10.30:8000 | Splunk accessible ✅ |

### Phase 3 — Log Forwarding

| Step | Action | Result |
|------|--------|--------|
| 1 | Installed Sysmon v15.20 | SwiftOnSecurity config applied |
| 2 | Installed Splunk Universal Forwarder | Pointing to .30:9997 |
| 3 | Created inputs.conf | Security + System + Sysmon logs |
| 4 | Changed to LocalSystem account | Sysmon log permissions fixed |
| 5 | Verified log flow | 2,167 Sysmon + 4,042 Windows events ✅ |

### Phase 4 — Detection Rules and Dashboard

| Step | Action | Result |
|------|--------|--------|
| 1 | Wrote 4 SPL detection rules | All mapped to MITRE ATT&CK |
| 2 | Tested rules with real data | Results confirmed |
| 3 | Saved as scheduled alerts | 5-minute intervals |
| 4 | Built SOC dashboard | 3 panels in Dashboard Studio |

---

## 🔍 Detection Rules

### Rule 1 — Brute Force Login Detection | `T1110`

```spl
index=windows EventCode=4625
| bucket _time span=5m
| stats count as failed_logins by _time, Account_Name
| where failed_logins >= 2
| eval severity="HIGH"
| eval mitre_technique="T1110 - Brute Force"
| table _time, Account_Name, failed_logins, severity, mitre_technique
```

**Detects:** Multiple failed login attempts (EventCode 4625) within a 5-minute window.

---

### Rule 2 — PowerShell Execution Detection | `T1059.001`

```spl
index=sysmon EventCode=1
| rex field=_raw "Image: (?<Image>[^\n]+)"
| rex field=_raw "CommandLine: (?<CommandLine>[^\n]+)"
| search Image="*powershell*" OR CommandLine="*EncodedCommand*" OR CommandLine="*bypass*"
| eval severity="HIGH"
| eval mitre_technique="T1059.001 - PowerShell"
| table _time, ComputerName, Image, CommandLine, severity, mitre_technique
| sort -_time
```

**Detects:** PowerShell processes and encoded command execution used by malware and attackers.

---

### Rule 3 — Suspicious Process Creation | `T1059`

```spl
index=sysmon EventCode=1
| rex field=_raw "Image: (?<Image>[^\n]+)"
| rex field=_raw "CommandLine: (?<CommandLine>[^\n]+)"
| search Image="*cmd.exe*" OR Image="*whoami*"
| eval severity=case(
    Image LIKE "%whoami%","HIGH",
    Image LIKE "%cmd.exe%","MEDIUM",
    1=1,"LOW")
| eval mitre_technique="T1059 - Command and Scripting"
| table _time, ComputerName, Image, CommandLine, severity, mitre_technique
| sort -_time
```

**Detects:** Post-exploitation commands — whoami, ipconfig, net user — used to enumerate the system.

---

### Rule 4 — Network Connection Detection | `T1046`

```spl
index=sysmon EventCode=3
| rex field=_raw "Image: (?<Image>[^\n]+)"
| rex field=_raw "DestinationIp: (?<DestinationIp>[^\n]+)"
| rex field=_raw "DestinationPort: (?<DestinationPort>[^\n]+)"
| where isnotnull(DestinationIp)
| stats count as connections by Image, DestinationIp, DestinationPort
| eval severity=case(
    DestinationPort="4444","CRITICAL",
    DestinationPort="443" OR DestinationPort="80","LOW",
    1=1,"MEDIUM")
| eval mitre_technique="T1046 - Network Service Scanning"
| table Image, DestinationIp, DestinationPort, connections, severity, mitre_technique
| sort -connections
```

**Detects:** Outbound network connections — catches malware C2 communication and port scanning.

---

## 🎯 MITRE ATT&CK Coverage

| Technique ID | Name | Rule | Severity | Events Detected |
|-------------|------|------|----------|-----------------|
| T1110 | Brute Force | Failed Login Detection | 🔴 HIGH | 6 events |
| T1059.001 | PowerShell | PowerShell Execution | 🔴 HIGH | 357 events |
| T1059 | Command Scripting | Process Creation | 🟠 MEDIUM-HIGH | 311 events |
| T1046 | Network Scanning | Network Connection | 🟡 MEDIUM | 2 events |

---

## 📸 Project Screenshots

### Lab Infrastructure
| Screenshot | Description |
|-----------|-------------|
| ![](screenshots/01_three_vms_running.png) | All 3 VMs running simultaneously on SOC-Lab network |
| ![](screenshots/02_installing_windows.png) | Windows 10 Pro installation inside VirtualBox |
| ![](screenshots/03_installing_kali.png) | Kali Linux graphical installer |
| ![](screenshots/04_vm1_done_snapshot.png) | VM 1 snapshot saved after clean install |
| ![](screenshots/24_virtualbox_guest_additions.png) | VirtualBox Guest Additions installed on Windows VM |

### Network Configuration
| Screenshot | Description |
|-----------|-------------|
| ![](screenshots/05_set_static_ip_windows.png) | Static IP 192.168.10.10 set on Windows VM |
| ![](screenshots/06_set_static_ip_kali.png) | Static IP 192.168.10.20 set on Kali VM |
| ![](screenshots/07_set_static_ip_ubuntu.png) | Static IP 192.168.10.30 set on Ubuntu VM |
| ![](screenshots/08_ping_kali_to_ubuntu.png) | Kali successfully pinging Windows VM |
| ![](screenshots/09_ping_ubuntu_to_windows.png) | Ubuntu successfully pinging Windows VM |
| ![](screenshots/23_ubuntu_kali_windows_snapshot.png) | All 3 VMs snapshot — Network-Configured |

### Splunk Installation and Access
| Screenshot | Description |
|-----------|-------------|
| ![](screenshots/10_internet_for_ubuntu_splunk.png) | Ubuntu VM with internet access for Splunk download |
| ![](screenshots/11_splunk_installing.png) | Splunk Enterprise installation in progress |
| ![](screenshots/12_splunk_login_windows_vm.png) | Splunk web UI accessed from Windows VM browser |

### Live Log Ingestion
| Screenshot | Description |
|-----------|-------------|
| ![](screenshots/13_sysmon_logs_splunk.png) | 2,167 Sysmon events in Splunk (index=sysmon) |
| ![](screenshots/14_soc_lab_index_windows.png) | 4,042 Windows Security events (index=windows) |

### Detection Rules in Action
| Screenshot | Description |
|-----------|-------------|
| ![](screenshots/15_rule1_bruteforce_T1110.png) | Rule 1 — Brute force detection (T1110) — 6 events |
| ![](screenshots/16_rule2_powershell_T1059.png) | Rule 2 — PowerShell detection (T1059.001) — 357 events |
| ![](screenshots/17_rule3_process_T1059.png) | Rule 3 — Process creation detection (T1059) — 311 events |
| ![](screenshots/18_rule4_network_T1046.png) | Rule 4 — Network connection detection (T1046) |
| ![](screenshots/19_all_alerts_saved.png) | All 4 detection alerts saved with 5-minute schedule |
| ![](screenshots/20_detect_T1110_alert.png) | DETECT-T1110-BruteForce alert firing in Splunk |

### SOC Dashboard
| Screenshot | Description |
|-----------|-------------|
| ![](screenshots/21_soc_dashboard.png) | SOC Detection Dashboard — Panel view 1 |
| ![](screenshots/22_soc_dashboard_2.png) | SOC Detection Dashboard — Panel view 2 |

---

## ⚡ Challenges and Solutions

| # | Challenge | Root Cause | Solution |
|---|-----------|-----------|----------|
| 1 | Ubuntu had no internet | Internal Network is intentionally isolated | Switched to NAT temporarily, fixed DNS with 8.8.8.8, downloaded Splunk, switched back |
| 2 | Splunk download returned HTML | Authenticated URL required — direct links expire | Used authenticated link with token copied directly from Splunk website after login |
| 3 | netplan config failing | File permissions too open, wrong YAML syntax | Created /etc/netplan directory manually, used chmod 600, correct 2-space indentation |
| 4 | Shared folder not working | Guest Additions v6.0.0 too old for Ubuntu 26.04 | Used NAT download method instead — simpler and more reliable |
| 5 | 0 Sysmon events in Splunk | Forwarder lacked permission to read Sysmon logs | Changed forwarder to LocalSystem via sc.exe config SplunkForwarder obj= LocalSystem |
| 6 | SPL rules returning 0 results | Sysmon fields stored in raw _raw field not parsed | Used rex field=_raw with regex to extract Image and CommandLine from raw event text |
| 7 | Forwarder login failing | Forwarder has separate admin account from Splunk | Reinstalled with SPLUNKPASSWORD parameter and configured outputs.conf directly |

---

## 📝 Key Contributions

- Built a 3-VM isolated SOC lab (Windows 10, Kali Linux, Ubuntu/Splunk) on an internal VirtualBox network simulating real enterprise attack and detection scenarios

- Deployed Splunk Enterprise SIEM ingesting 6,000+ live events from Sysmon and Windows Security logs forwarded in real time via Splunk Universal Forwarder

- Authored 4 custom Splunk SPL detection rules mapped to MITRE ATT&CK techniques T1110, T1059, T1046 with automated scheduled alerting every 5 minutes

- Configured Sysmon v15.20 with SwiftOnSecurity config on Windows endpoint to collect process creation, network connection, and file system telemetry

- Built a SOC detection dashboard in Splunk Dashboard Studio visualizing failed logins, process activity, and network connections across the lab environment in real time

---

## 📄 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

## 👤 Author

**Rajkumar Jangam**

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?logo=linkedin)](https://linkedin.com/in/rajkumar-jangam-559827228)
[![GitHub](https://img.shields.io/badge/GitHub-raj--1819-black?logo=github)](https://github.com/raj-1819)
[![Portfolio](https://img.shields.io/badge/Portfolio-Visit-green)](https://raj-1819.github.io/Portfolio)

---

*Built as part of a cybersecurity portfolio to demonstrate SOC analyst skills in threat detection, SIEM operations, log analysis, and MITRE ATT&CK framework application.*
