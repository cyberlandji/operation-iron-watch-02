🔴Primary Evidence — SOC Perspective (soc/)

This directory contains authoritative evidence used by the SOC during detection and analysis.

Included Evidence

Network reconnaissance visibility

ICMP and Nmap traffic observed in Graylog

Web enumeration detection

Repeated HTTP 404 responses from a single source

Aggregated threshold-based alert triggering

Alert validation

Abnormal web activity detected prior to compromise

Log-level confirmation

Raw HTTP access events supporting the alert

Purpose

This evidence demonstrates:

Early-stage attack detection

Valid alert logic

Accurate identification of suspicious behavior

Detection occurring before successful compromise

All detection conclusions in Iron Watch 02 are based solely on this SOC-side telemetry.

🟡 Supporting Evidence — Attacker Context (attacker-context/)

This directory contains contextual evidence collected from the attacker system.

Included Evidence

ICMP host discovery (ping)

Network/service enumeration (nmap)

Manual web enumeration (curl, wget)

SSH brute-force execution (hydra)

Purpose

Attacker-side evidence is included to:

Correlate SOC alerts with real attacker actions

Demonstrate causality behind observed log events

Support learning and transparency

⚠️ Note:
Detection and alerting decisions are not based on attacker-side data.
This evidence is provided strictly for correlation and educational purposes.

🔗 Correlation Summary

The collected evidence supports the following attack progression:

Reconnaissance

ICMP and Nmap activity observed by SOC

Web Enumeration

Manual probing of common paths

Abnormal increase in HTTP 404 responses

Alert triggered based on defined threshold

SSH Brute Force

Authentication attempts confirmed locally in auth.log

SSH compromise occurred after web alert detection

SSH logs were not ingested into the SIEM at incident time

This sequence confirms that early indicators were detected, while later-stage visibility was limited by telemetry coverage.

📌 Key Takeaway

The evidence in this directory demonstrates that:

Detection logic for reconnaissance and web enumeration was effective

Alerts were triggered before attacker success

Gaps in log ingestion directly impacted SOC visibility

Incident reconstruction remains possible through correlation of available data

These findings directly inform the conclusions and improvements documented in 08-lessons-learned.
