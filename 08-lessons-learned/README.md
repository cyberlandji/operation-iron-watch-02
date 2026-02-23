📘 08-lessons-learned — Detection Gaps, Correlation & Improvements
Purpose

This section documents the analytical conclusions of Iron Watch 02, focusing on:

What was detected successfully

What was partially or not detected

How events were correlated across the attack lifecycle

Which improvements are required to strengthen future detection capability

The goal is to transform observed events into actionable SOC knowledge.

🔗 Incident Correlation Summary

The incident followed a clear and coherent progression:

Reconnaissance

ICMP probing and Nmap scanning from a single external source

Activity observed and logged within the SIEM

Web Enumeration

Manual probing of common paths and files

Abnormal volume of HTTP 404 responses

Threshold-based alert triggered successfully

SSH Brute Force

Dictionary-based authentication attempts

Multiple failed logins followed by a successful SSH login

SSH activity confirmed locally on the target host

The SOC was able to detect and alert on early-stage attacker behavior prior to successful compromise.
However, visibility into the final attack stage was limited due to telemetry constraints.

🧩 MITRE ATT&CK Mapping

Observed attacker behavior was mapped to the MITRE ATT&CK framework to contextualize detection coverage.

Reconnaissance

Active Scanning

ICMP host discovery

Network and service enumeration via Nmap

Discovery

Network Service Discovery

Identification of exposed HTTP and SSH services

Initial Access

Brute Force

SSH password-based brute-force attack resulting in initial access

Impact

Not observed at SOC level

No post-exploitation activity

No persistence, privilege escalation, or lateral movement

This mapping highlights that pre-compromise activity was observable, while post-compromise visibility depended on additional log ingestion.

🚨 Detection Coverage Assessment
Successfully Detected

Network reconnaissance activity

Web enumeration behavior

Abnormal HTTP 404 patterns

Suspicious activity originating from a consistent source IP

Alert triggered before successful compromise

Partially Detected

SSH brute-force activity

Authentication attempts existed locally

Not visible in SIEM at incident time

Not Detected at SOC Level

Successful SSH authentication

Session establishment events

The absence of SSH telemetry in the SIEM directly limited the SOC’s ability to confirm impact from a centralized view.

⚠️ Identified Gaps
Log Ingestion Gap

SSH authentication logs (auth.log) were present on the target system

These logs were not ingested into the SIEM

Resulted in blind spots during post-alert investigation

Time Synchronization

Graylog operated in Europe/London

Target host operated in Europe/Berlin

A consistent one-hour offset was observed

While event ordering and correlation remained possible, this emphasized the importance of time normalization for multi-source investigations.

🔇 About Benign Background Activity

Iron Watch 02 was executed with limited benign background activity by design.

This controlled approach allowed for:

Clear distinction between baseline and malicious behavior

Accurate threshold tuning

Reduced false positives during early detection validation

The absence of heavy benign noise is intentional, not a limitation.
Increased background activity and mixed traffic will be introduced in future operations to further challenge correlation logic.

🛠️ Recommended Improvements (Post-Incident)

The following actions are identified for future operations:

Ingest SSH authentication logs into the SIEM

Normalize timestamps across all data sources (preferably UTC)

Create correlation rules linking:

SSH authentication failures → successful logins

Expand detection logic beyond web-layer indicators

Introduce higher levels of benign background activity

Improve end-to-end visibility for post-compromise analysis

These improvements naturally lead into the scope of Iron Watch 03.

🎯 Key Takeaways

Iron Watch 02 demonstrates that:

Early-stage attacker behavior can be detected effectively

Detection quality is limited by telemetry coverage, not alert logic alone

Controlled environments are critical for building reliable baselines

Visibility gaps provide valuable learning opportunities

Honest documentation strengthens SOC maturity

Iron Watch 02 serves as a foundational detection case study, establishing the groundwork for more advanced detection and response scenarios.
