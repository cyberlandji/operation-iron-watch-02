# 04 – Monitoring Strategy
Perspective: Blue Team / SOC Analyst
Goal: Visibility, Detection, and Investigation
Focus: Host + Network + Application Telemetry

MONITORING OBJECTIVES:

- Achieve full visibility of attacker activity on web-arm01
- Detect reconnaissance and enumeration behaviors
- Correlate host, network, and application-level events
- Generate actionable alerts for analyst investigation
- Support evidence-driven conclusions

MONITORING OBJECTIVES:

- Achieve full visibility of attacker activity on web-arm01
- Detect reconnaissance and enumeration behaviors
- Correlate host, network, and application-level events
- Generate actionable alerts for analyst investigation
- Support evidence-driven conclusions


MONITORING FLOW:

redforge-01 (Attacker)
        |
        v
web-arm01 (Target)
        |
        |  Logs / Events / Telemetry
        v
soc-core03 (SIEM & Detection)
        |
        v
Safeguard Host (Analyst)
