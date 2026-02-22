# STEP 3 — Log Ingestion (Minimal & Controlled)

## Purpose

This step introduces log ingestion into the SOC for the first time.
The objective is **visibility only** — not parsing perfection, not alerting,
and not dashboards.

Logs are ingested **incrementally** and **deliberately**, following SOC
operational discipline.

---

## Scope

- Operation: Iron Watch 02
- Phase: Initial ingestion preparation
- SOC Node: soc-core03
- Log Sources:
    - soc-core03 (local)
    - web-arm01 (remote)
- Detection logic: NONE
- Alerting: DISABLED
- Dashboards: NONE

---

## Design Rules (Strict)

- One source at a time
- No detection rules
- No correlation
- No dashboards
- Validate each step before proceeding
- If ingestion is unreliable, STOP

---

## Ingestion Order

1. soc-core03 — local logs
2. web-arm01 — remote logs

---

## 1. soc-core03 — Local Log Readiness

### Why this matters

The SOC must first observe **itself**.
If local logs are not healthy, remote ingestion will fail silently later.

---

### Target Log Files

- /var/log/syslog
- /var/log/auth.log

---

### Verify Log Presence

```bash
ls -lh /var/log/syslog /var/log/auth.log
```

### Expected

- Files exist
- File sizes are non-zero
- Readable by root

📸 Evidence:

- Log file listing

---

### Verify Live Log Activity

```
sudo tail-n20 /var/log/syslog
sudo tail-n20 /var/log/auth.log
```

### Expected

- New entries appear
- Timestamps are current
- No obvious corruption

📸 Evidence:

- Sample log entries

---

### Outcome (Local)

- SOC node logs are present
- Logs are actively written
- SOC telemetry is reliable

Proceed only if all checks pass.

---

## 2. web-arm01 — Remote Log Readiness

### Why this matters

Logs must be valid and active **at the source** before forwarding.

Forwarding broken or empty logs leads to false confidence.

---

### Target Log Files (web-arm01)

- /var/log/apache2/access.log
- /var/log/apache2/error.log
- /var/log/auth.log
- /var/log/syslog

---

### Verify Log Presence (run on web-arm01)

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

### Generate Benign Activity (web-arm01)

```
curl http://localhost
curl http://localhost/nonexistent-page
```

Then verify:

```
sudo tail-n10 /var/log/apache2/access.log
sudo tail-n10 /var/log/apache2/error.log
```

### Expected

- HTTP 200 and 404 responses logged
- New entries appear immediately
- Timestamps correct

📸 Evidence:

- Apache access and error logs

---

## 3. Transport Method (Defined, Not Activated)

### Chosen Approach

- File-based log forwarding
- Simple and transparent
- No agent sprawl
- Reproducible

### Status

- Transport method SELECTED
- Transport method NOT CONFIGURED yet

Activation is deferred to STEP 4.

---

## What Is Intentionally NOT Done

- No log shipping service started
- No indexing
- No parsing rules
- No alerts
- No dashboards

This protects clarity and avoids false assumptions.

---

## Outcome of STEP 3

At the end of this step:

- Local SOC logs are confirmed healthy
- Remote target logs are confirmed healthy
- Log paths are validated
- Benign activity is observable at the source
- Environment is ready for ingestion activation

➡️ Next step:

STEP 4 — Log Ingestion Activation & Validation

---

## Notes

- Missing logs here invalidate all downstream work
- Do not postpone ingestion issues
- Detection quality depends on ingestion correctness
