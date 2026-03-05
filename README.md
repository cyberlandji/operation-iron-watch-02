# 🔐 Operation Iron Watch 02
## Graylog SIEM — Web Enumeration Detection & Visibility Gaps

[![Status](https://img.shields.io/badge/status-complete-brightgreen)](https://github.com/cyberlandji/operation-iron-watch-02)
[![Lab Series](https://img.shields.io/badge/series-Operation%20Iron%20Watch-blue)](https://github.com/cyberlandji)
[![Focus](https://img.shields.io/badge/focus-Graylog%20SIEM%20%26%20Detection-teal)](https://github.com/cyberlandji/operation-iron-watch-02)

---

## 📌 Overview

Operation Iron Watch 02 introduces **Graylog as a SIEM** and validates baseline detection capability against a simulated external attacker targeting a publicly exposed web server.

IW02 successfully detected web enumeration activity via HTTP 404 spike alerting. However, it also exposed a critical blind spot: a **real SSH compromise occurred and was completely invisible on the SIEM** because `auth.log` was never ingested into Graylog.

> IW02 proves that a SIEM is only as good as what it ingests. Visibility gaps are security gaps.

---

## 🎯 Objectives

- Establish and validate baseline network and web activity
- Detect web reconnaissance and enumeration via SIEM alerting
- Validate threshold-based detection logic before successful compromise
- Identify visibility gaps caused by incomplete log ingestion
- Practice structured incident reconstruction and correlation

---

## 🏗️ Lab Infrastructure

| Host | IP | Role |
|------|----|------|
| `soc-core04` (VM) | 192.168.0.5 | Graylog SIEM |
| `web-arm01` (Raspberry Pi) | 192.168.0.9 | Apache2 web server — target |
| Safeguard Host | 192.168.0.7 | Physical host / admin access |

> Both `soc-core04` and `web-arm01` were on the same flat LAN (192.168.0.0/24) — no segmentation. This is a key architectural gap addressed in IW03.

---

## 🛠️ Detection Stack

| Tool | Host | Role |
|------|------|------|
| Graylog | soc-core04 | SIEM — log ingestion and alerting |
| Apache2 | web-arm01 | Web server — `access.log` source |
| rsyslog | web-arm01 | Log forwarding to Graylog |

---

## 🔍 Detection — What Worked

### HTTP 404 Spike Rule (Web Enumeration)

Graylog alert triggered on abnormal HTTP 404 response rate from a single source IP within a time window — consistent with automated web enumeration tools scanning for valid paths and resources.

| Signal | Threshold | Result |
|--------|-----------|--------|
| HTTP 404 responses per source IP | Spike above baseline | ✅ Alert fired correctly |

> Detection confirmed: early-stage web reconnaissance is detectable via access log analysis before any successful exploitation occurs.

---

## ⚠️ The Blind Spot — What Failed

### SSH Compromise Was Invisible

During IW02, a **real SSH brute-force succeeded** and the attacker gained shell access to `web-arm01`. Graylog generated **zero alerts** for this event.

**Root cause:** `auth.log` was never ingested into Graylog. The log pipeline only covered `access.log`.

| Event | Logged on Host | In Graylog | Alert Fired |
|-------|---------------|------------|-------------|
| Web enumeration (HTTP 404s) | ✅ | ✅ | ✅ |
| SSH brute-force attempts | ✅ | ❌ | ❌ |
| SSH successful login | ✅ | ❌ | ❌ |
| Post-compromise activity | ✅ | ❌ | ❌ |

> The attacker moved freely on `web-arm01` while Graylog watched HTTP traffic. This is a textbook example of incomplete telemetry creating a false sense of security.

---

## 🗺️ MITRE ATT&CK Mapping

| Technique | Tactic | Status |
|-----------|--------|--------|
| Active Scanning — Web Enumeration | Reconnaissance | ✅ Detected |
| Brute Force — Password Spraying | Credential Access | ⚠️ Occurred — Not Detected |
| Valid Accounts — SSH | Initial Access | ⚠️ Succeeded — Not Detected |

---

## 📂 Structure

```
operation-iron-watch-02/
├── 00-overview/              # Operation summary and intent
├── 01-architecture/          # Network topology and host details
├── 02-scope/                 # Lab scope and boundaries
├── 03-threat-scenario/       # Attacker scenario and TTPs
├── 04-monitoring-strategy/   # Detection logic and alert rules
├── 05-lab-setup/             # Host configurations
├── 06-detection-use-cases/   # Alert rules and detection documentation
├── 07-evidences/             # Screenshots and log samples
├── 08-lessons-learned/       # Gap analysis and key takeaways
├── 09-postmortems/           # SSH compromise post-mortem
└── README.md
```

---

## 🏁 Key Takeaways

- **Early detection is possible** — web enumeration was caught before any successful compromise
- **Incomplete log ingestion is a critical gap** — if it is not ingested, it does not exist from the SIEM's perspective
- **Flat network architecture is dangerous** — once inside `web-arm01`, an attacker could move freely toward LAN hosts
- **A SIEM without full telemetry coverage gives a false sense of security**

---

## 🔗 Iron Watch Series

| Episode | Focus | Status |
|---------|-------|--------|
| [Iron Watch 01](https://github.com/cyberlandji/operation-iron-watch-01) | Foundational SOC — Snort IDS, manual correlation | ✅ Complete |
| **Iron Watch 02** | **Graylog SIEM — web enumeration detection, SSH compromise invisible** | ✅ Complete |
| [Iron Watch 03](https://github.com/cyberlandji/operation-iron-watch-03) | DMZ hardening, log pipeline, DDoS detection suite | 🔄 In Progress |
| Iron Watch 04 | Attack validation — Kali recon & initial access against IW03 | 🔜 Planned |

---

## 👤 Author

**cyberlandji** — Blue Team Practitioner | ISC2 CC | CompTIA Security+ (in progress)

Portfolio: [cyberlandji.com](https://cyberlandji.com) · GitHub: [github.com/cyberlandji](https://github.com/cyberlandji)
