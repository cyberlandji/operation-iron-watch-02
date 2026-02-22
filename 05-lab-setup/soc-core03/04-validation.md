# STEP 4 — Log Ingestion Activation & Validation

## Purpose

This step **activates log ingestion** and validates **end-to-end visibility**
from the monitored asset (`web-arm01`) to the SOC node (`soc-core03`).

The goal is simple and strict:

- Logs must arrive
- Logs must be readable
- Timestamps must be correct
- No detection, alerting, or dashboards yet

If ingestion is unreliable, **STOP here**.

---

## Scope

- Operation: Iron Watch 02
- Phase: Ingestion activation
- SOC Node: soc-core03
- Source Node: web-arm01
- Transport: File-based forwarding
- Detection logic: NONE
- Alerting: DISABLED

---

## Design Rules (Still Enforced)

- No parsing optimization
- No correlation
- No alerts
- No dashboards
- Validate with real log lines
- One failure = pause progression

---

## 1. Ingestion Method Overview

### Chosen Method

- Secure file-based log forwarding
- Uses system-native logging mechanisms
- Minimal complexity
- Fully observable and debuggable

This mirrors many real SOC entry-level pipelines.

---

## 2. Configure web-arm01 Log Forwarding

### Why this matters

Log forwarding must be **explicit, intentional, and auditable**.
Hidden magic leads to blind spots.

---

### Configure rsyslog on web-arm01

> Run the following **on web-arm01**
> 

Create a dedicated forwarding configuration:

```bash
sudo tee /etc/rsyslog.d/60-soc-forwarding.conf << 'EOL'
# Forward selected logs to SOC node
*.* @@soc-core03:514
EOL
```

Restart rsyslog:

```
sudo systemctlrestart rsyslog
sudo systemctl status rsyslog
```

### Expected

- rsyslog service active
- No configuration errors
- No service crash loops

📸 Evidence:

- rsyslog service status

---

## 3. Prepare soc-core03 to Receive Logs

### Why this matters

The SOC node must explicitly listen for incoming logs.

Nothing should arrive silently.

---

### Enable rsyslog Listener (soc-core03)

Edit rsyslog configuration:

```
sudosed-i's/^#module(load="imudp")/module(load="imudp")/' /etc/rsyslog.conf
sudosed-i's/^#input(type="imudp" port="514")/input(type="imudp" port="514")/' /etc/rsyslog.conf
sudosed-i's/^#module(load="imtcp")/module(load="imtcp")/' /etc/rsyslog.conf
sudosed-i's/^#input(type="imtcp" port="514")/input(type="imtcp" port="514")/' /etc/rsyslog.conf
```

Restart rsyslog:

```
sudo systemctlrestart rsyslog
sudo systemctl status rsyslog
```

Verify listening ports:

```
sudo ss-tulpn |grep514
```

### Expected

- soc-core03 listening on TCP/UDP 514
- rsyslog running without errors

📸 Evidence:

- Listening ports
- rsyslog status

---

## 4. Validate Log Arrival (End-to-End)

### Generate Test Events on web-arm01

```
logger"IW02 TEST — web-arm01 log forwarding validation"
```

---

### Verify Logs on soc-core03

```
sudo tail-n30 /var/log/syslog
```

### Expected

- Test message appears
- Source hostname is web-arm01
- Timestamp is correct
- No mangled or missing fields

📸 Evidence:

- Received test log line

---

## 5. Validate Application Log Flow

### Generate Web Activity (web-arm01)

```
curl http://localhost
curl http://localhost/404-test
```

---

### Verify Arrival on soc-core03

```
sudogrep"GET /" /var/log/syslog | tail
```

### Expected

- HTTP requests visible
- Source identified as web-arm01
- Log volume matches activity

📸 Evidence:

- Apache log entries visible on soc-core03

---

## 6. Noise Sanity Check

### Why this matters

Before detection engineering, we must understand **log volume and noise**.

```
sudowc-l /var/log/syslog
sleep30
sudowc-l /var/log/syslog
```

### Expected

- Log count increases gradually
- No flooding
- No duplicated spam

📸 Evidence:

- Log growth observation

---

## 7. Failure Conditions (STOP Rules)

STOP and troubleshoot if:

- Logs do not arrive
- Hostnames are missing
- Timestamps are incorrect
- rsyslog crashes or restarts
- Log volume explodes unexpectedly

Do **not** proceed to detection.

---

## Outcome of STEP 4

At the end of this step:

- soc-core03 receives logs from web-arm01
- Log flow is verified end-to-end
- Timestamps and sources are correct
- Ingestion pipeline is trustworthy
