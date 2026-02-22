# 

# STEP 3 — Log Ingestion (Minimal & Controlled)

## Purpose

This step introduces **log ingestion** into the SOC for the first time.
The objective is **visibility only** — not parsing perfection, not alerting,
and not dashboards.

We ingest logs **incrementally**, starting with the SOC node itself,
then extending to the monitored asset.

---

## Scope

- Operation: Iron Watch 02
- Phase: Initial ingestion
- SOC Node: soc-core03
- Sources:
  - soc-core03 (local logs)
  - web-arm01 (remote logs)
- Detection logic: NONE
- Alerting: DISABLED

---

## Design Rules (Strict)

- One source at a time
- No detection rules
- No correlation
- No dashboards
- Validate ingestion before moving forward
- If ingestion is unreliable, STOP

This mirrors real SOC rollout discipline.

---

## Ingestion Order

1. soc-core03 → self-ingestion (local)
2. web-arm01 → remote ingestion

---

## 1. Ingesting soc-core03 Logs (Local)

### Why start here

The SOC must first observe **itself**.
If local ingestion fails, remote ingestion will fail silently later.

---

### Target Log Files

- /var/log/syslog
- /var/log/auth.log

These logs already exist locally and require no forwarding yet.

---

### Validation — Log Presence

```bash
ls -lh /var/log/syslog /var/log/auth.log

### Expected

- Files exist
- File size increases over time
- Readable by root

📸 Evidence:

- Log file listing

---

### Validation — Live Log Activity

```
sudo tail-n20 /var/log/syslog
sudo tail-n20 /var/log/auth.log
```

### Expected

- New entries appear
- Timestamps are correct
- No obvious parsing errors

📸 Evidence:

- Sample log entries

---

### Outcome (Local)

- Logs are confirmed present
- Logs are actively written
- SOC node telemetry is available

➡️ Proceed only if all checks pass.

---

## 2. Preparing web-arm01 for Log Forwarding

### Why this matters

Remote logs must be **stable at the source** before forwarding.

Forwarding broken or empty logs leads to false assumptions.

---

### Target Log Files on web-arm01

- /var/log/apache2/access.log
- /var/log/apache2/error.log
- /var/log/auth.log
- /var/log/syslog

---

### Validation — Log Presence (on web-arm01)

Run on **web-arm01**:

```
ls-lh /var/log/apache2/access.log \
       /var/log/apache2/error.log \
       /var/log/auth.log \
       /var/log/syslog
```

### Expected

- All files exist
- Apache logs contain entries
- Auth and system logs are active

📸 Evidence:

- Log file listing on web-arm01

---

### Validation — Generate Benign Activity

On **web-arm01**, generate safe activity:

```
curl http://localhost
curl http://localhost/nonexistent-page
```

Then re-check:

```
sudo tail-n10 /var/log/apache2/access.log
sudo tail-n10 /var/log/apache2/error.log
```

### Expected

- New HTTP entries appear
- 200 and 404 responses visible
- Timestamps correct

📸 Evidence:

- Apache access and error log entries

---

## 3. Transport Method (Defined, Not Configured Yet)

At this step, the **transport mechanism is selected but not yet activated**.

Chosen approach for Iron Watch 02:

- File-based forwarding
- Simple, transparent, reproducible
- No agent sprawl

Actual configuration is deferred to **STEP 4 (Validation & Activation)**.

---

## What Is Intentionally NOT Done Yet

- No log shipping service started
- No index created
- No parsing rules
- No alerts
- No dashboards

This is intentional and protects clarity.

---

## Outcome of STEP 3

At the end of this step:

- Local SOC logs are confirmed active
- Remote target logs are confirmed active
- Log paths are validated
- Benign activity is observable at the source
- Ingestion readiness is confirmed
