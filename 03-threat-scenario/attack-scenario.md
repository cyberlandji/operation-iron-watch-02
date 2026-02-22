#!/bin/bash
###############################################################################
# FILE: 03-threat-scenario/attack-scenario.md
# PROJECT: Operation Iron Watch 02
# PURPOSE: Attacker scenario definition (locked for IW02)
###############################################################################

cat << 'EOF' > attack-scenario.md
# Attacker Scenario — Iron Watch 02

## Overview
This document describes the attacker behavior scenario used to frame
detection activities in **Iron Watch 02**.

The scenario is intentionally **partially observable**.  
Only selected attacker actions are detectable in IW02, while others remain
undetected by design. These gaps directly motivate **Iron Watch 03**.

---

## Assumptions
- The target environment exposes a public web service (Apache) and SSH access
- The web server is intentionally **not fully hardened**
- SSH credentials on `web-arm01` are weak
- The attacker has no prior authentication or insider access

---

## Phase 1 — Network Discovery (Not Detected)
The attacker begins with basic network reconnaissance to identify live hosts
and exposed services.

Typical activity includes:
- ICMP ping scanning
- TCP port scanning (e.g. via nmap)

Through this activity, the attacker identifies that the target host
(`web-arm01`) is reachable and exposes:
- an HTTP service (Apache)
- an SSH service

> Detection status: ❌ Not detected in IW02  
> Reason: No network-level telemetry or baselining implemented at this stage

---

## Phase 2 — Service Identification (Not Detected)
Based on scan results, the attacker confirms:
- the presence of a web server
- an accessible SSH service

The attacker prioritizes the web service as the initial access vector.

> Detection status: ❌ Not detected in IW02  
> Reason: Service discovery telemetry not in scope

---

## Phase 3 — Web Enumeration (Detected)
The attacker attempts to enumerate the web application by guessing:
- undocumented paths
- hidden files
- potential downloadable resources (e.g. `/ip.txt`, `/backup/`, `/admin/`)

This activity results in a **high volume of HTTP 404 (Not Found) responses**
within a short time window.

> Detection status: ✅ Detected in IW02  
> Detection method:
> - Apache access log ingestion
> - Parsed fields (`client_ip`, `http_status`)
> - Aggregation-based detection on abnormal 404 volume

This phase represents the **only fully detected attacker behavior in IW02**.

---

## Phase 4 — Failed Web Exploitation (Not Detected)
The attacker fails to locate exploitable web resources or sensitive files.

No successful web-layer compromise occurs.

> Detection status: ❌ Not detected  
> Reason: No exploitation attempt succeeded, no additional web baselines defined

---

## Phase 5 — Pivot to SSH Brute Force (Not Detected)
After failing at the web layer, the attacker pivots to the SSH service.

Using a dictionary-based brute force attack against weak credentials on
`web-arm01`, the attacker successfully authenticates.

> Detection status: ❌ Not detected in IW02  
> Reasons:
> - No SSH authentication baseline
> - No detection rules for login failures or brute force behavior

---

## Phase 6 — Compromise Outcome
The attacker gains authenticated access to `web-arm01` via SSH.

This compromise highlights multiple defensive gaps:
- missing SSH monitoring
- weak credentials
- incomplete service baselining

---

## Scope Clarification (Important)
Iron Watch 02 intentionally focuses on **early detection with limited baselines**.

Detected:
- Web reconnaissance via HTTP 404 enumeration

Not detected:
- Network scanning
- SSH brute force
- Post-compromise activity

These gaps are **intentional** and form the foundation for Iron Watch 03.

---

## Transition to Iron Watch 03
Iron Watch 03 is designed to address the consequences observed in this scenario:
- Establish missing baselines (HTTP 200, HTTP 403, SSH authentication)
- Detect brute force behavior
- Introduce response and hardening measures

---

## Status
**Locked — Iron Watch 02 attacker scenario finalized**
EOF

echo "✔ attack-scenario.md created and Iron Watch 02 scenario locked."
