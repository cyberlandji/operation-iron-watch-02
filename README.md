🛡️ Iron Watch 02 — Overview

Iron Watch 02 is a Blue Team–focused detection exercise designed to validate baseline visibility, early-stage attack detection, and incident correlation within a controlled SOC lab environment.

The operation simulates a realistic external attacker performing reconnaissance, web enumeration, and SSH brute-force activity against a publicly exposed web server. Detection efforts are centered on identifying abnormal behavior through log analysis and threshold-based alerting, rather than achieving full attacker containment.

🎯 Scope & Intent

The primary objectives of Iron Watch 02 are to:

Establish and validate baseline network and web activity

Detect reconnaissance and enumeration behavior using SIEM telemetry

Trigger and validate alerts prior to successful compromise

Identify visibility gaps caused by incomplete log ingestion

Practice structured incident reconstruction and correlation

This operation emphasizes detection accuracy and analytical reasoning over attack complexity.

🔇 About Benign Background Activity

Iron Watch 02 was executed with limited benign background activity by design.

The environment was intentionally kept controlled to:

Clearly distinguish baseline behavior from malicious activity

Reduce noise during initial detection logic validation

Prevent false positives while establishing alert thresholds

Enable precise correlation between attacker actions and observed telemetry

Limited benign noise in this phase is not a limitation, but a deliberate architectural choice aligned with early-stage SOC detection development.

Increased background activity and mixed benign/malicious traffic are intentionally deferred to future operations (e.g., Iron Watch 03), where correlation complexity and detection tuning will be further challenged.

📌 Key Takeaway

Iron Watch 02 demonstrates that:

Early indicators of compromise can be detected before attacker success

Effective detection relies on telemetry coverage, not only alert logic

Missing or incomplete log ingestion directly impacts SOC visibility

Controlled environments are essential for building reliable detection foundations

This operation serves as a foundational detection baseline, upon which future improvements and more complex scenarios will be built.
