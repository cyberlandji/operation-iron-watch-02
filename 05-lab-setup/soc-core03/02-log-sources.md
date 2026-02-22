# STEP 2 — Log Sources Definition (Iron Watch 02)

## Purpose

Before configuring log ingestion or detection logic, the SOC must clearly define
**what systems generate logs**, **which logs matter**, and **why they are collected**.

This step establishes **visibility scope**.
No ingestion, parsing, or alerts are configured yet.

---

## Scope

- Operation: Iron Watch 02
- Phase: Pre-ingestion
- SOC Node: soc-core03
- Objective: Define authoritative log sources and log types

---

## Design Principles

The following principles guide log selection:

- Collect logs that support **detection and investigation**
- Avoid unnecessary noise at early stages
- Prefer logs that reflect **realistic SOC data**
- Ensure each log source has a **clear purpose**

---

## Log Source Overview

| Source System | Role | Log Category |
|--------------|------|-------------|
| soc-core03 | SOC platform | System & security logs |
| web-arm01 | Target system | Web, auth, and system logs |

Only **two sources** are intentionally used at this stage to preserve clarity.

---

## 1. soc-core03 (SOC Platform)

### Role
`soc-core03` is the central analysis node.
Its own logs are required for **self-monitoring**, troubleshooting, and auditability.

### Log Types

#### System Logs
- `/var/log/syslog`
- `/var/log/kern.log`

**Purpose**
- Detect system instability
- Track service failures
- Investigate SOC-side issues

---

#### Authentication Logs
- `/var/log/auth.log`

**Purpose**
- Monitor SSH access attempts
- Detect brute-force or unauthorized access
- Validate SOC node integrity

---

### Collection Intent
- Logs are **locally available by default**
- Will later be indexed for self-visibility
- No alerting at this stage

📸 Evidence (later):
- Presence of log files
- Sample entries

---

## 2. web-arm01 (Monitored Asset)

### Role
`web-arm01` is the monitored system and simulated attack target.
It provides **application-layer and host-layer telemetry**.

---

### Log Types

#### Apache Access Logs
- `/var/log/apache2/access.log`

**Purpose**
- Track HTTP requests
- Detect scanning, enumeration, and abuse patterns
- Observe baseline vs malicious behavior

---

#### Apache Error Logs
- `/var/log/apache2/error.log`

**Purpose**
- Identify failed requests
- Detect misconfigurations
- Support web attack investigations

---

#### Authentication Logs
- `/var/log/auth.log`

**Purpose**
- Detect SSH brute-force attempts
- Identify unauthorized login behavior
- Correlate access with attacker activity

---

#### System Logs
- `/var/log/syslog`

**Purpose**
- Capture system-level events
- Provide additional context during incidents

---

### Collection Intent
- Logs will be **forwarded to soc-core03**
- Transport method defined in STEP 3
- No parsing or alerting yet

📸 Evidence (later):
- Log file existence
- Sample benign entries

---

## Log Sources Explicitly Excluded (For Now)

The following are intentionally excluded at this phase:

- Firewall logs
- IDS/IPS alerts
- Endpoint agents
- Cloud telemetry
- Packet capture

**Reason**
Early complexity increases noise and reduces learning clarity.
These may be introduced in future operations.

---

## Trust Boundaries

- `soc-core03` is considered **trusted**
- `web-arm01` is considered **monitored but untrusted**
- Attacker systems are **not log sources**

This mirrors real SOC assumptions.

---

## Outcome of STEP 2

At the end of this step:

- Log sources are clearly identified
- Log file paths are known
- Collection intent is documented
- No data ingestion has occurred yet

This definition becomes the **contract** for STEP 3.

➡️ Next step:
STEP 3 — Log Ingestion (Minimal & Controlled)

---

## Notes

- No detection logic applied
- No dashboards created
- No alerts enabled
- Focus is on visibility planning, not action

---

END OF STEP 2
EOF
