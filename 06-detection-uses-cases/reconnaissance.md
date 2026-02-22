###############################################################################
# FILE: 06-detection-use-cases/reconnaissance.md
# PROJECT: Operation Iron Watch 02
# PURPOSE: Detection use case documentation — Reconnaissance phase
###############################################################################


# Detection Use Case: Web Reconnaissance via HTTP 404 Enumeration

## Overview
This detection use case focuses on identifying **potential web reconnaissance
or enumeration activity** against a publicly accessible Apache web server by
monitoring abnormal volumes of HTTP 404 responses over a short time window.

The objective is to detect **behavioral patterns**, not individual requests.

---

## Threat Context
During reconnaissance, attackers often attempt to discover:
- hidden directories
- undocumented endpoints
- application structure

This activity commonly generates a **high number of HTTP 404 (Not Found)**
responses in a short period of time.

---

## Scope
**In scope**
- Apache access logs from `web-arm01`
- HTTP 404 responses
- Client-side behavior grouped by source IP

**Out of scope (intentionally)**
- HTTP 200 baseline behavior
- HTTP 403 forbidden responses
- SSH authentication attempts
- Background noise and mixed benign traffic

> Note: These baselines are intentionally deferred to **Iron Watch 03** to
> demonstrate the operational consequences of incomplete baselining.

---

## Data Source
- **Host:** web-arm01
- **Service:** Apache HTTP Server
- **Log Type:** access.log
- **Ingestion Path:** rsyslog → Graylog (TCP, facility local6)

---

## Parsed Fields Used
The following fields are extracted via Graylog pipeline processing:
- `client_ip`
- `http_status`
- `http_method`

Only structured fields are used for detection logic.

---

## Detection Logic
**Event Type:** Filter & Aggregation

**Filter**
```text
http_status:404
**Aggregation**

- Function: `count()`
- Group by: `client_ip`
- Time window: 5 minutes

**Threshold**

```
count(http_status=404) > 10
```

An event is generated when a single client IP exceeds the defined threshold

within the time window.

---

## Baseline Considerations

Baseline validation for this use case focuses **only on HTTP 404 frequency**.

- Normal behavior: occasional 404 responses
- Abnormal behavior: burst of 404 responses from the same client IP

No assumptions are made about other HTTP status codes or services at this stage.

---

## Validation Method

Detection was validated by generating repeated requests to non-existent

endpoints using `curl`, simulating basic enumeration behavior.

Example:

```
for iin {1..15};do
curl http://<web-arm01>/does-not-exist-$i
done
```

---

## Outcome

- Detection successfully triggered
- Events correctly grouped by `client_ip`
- Threshold respected
- Alert visible in Graylog **Alerts & Events**

---

## Limitations

This detection does not account for:

- Legitimate scanners
- Health checks
- Normal high-traffic applications
- Forbidden resource probing (403)

These gaps are acknowledged and will be addressed in **Iron Watch 03**.
