SCENARIO NARRATIVE:

An internal attacker (redforge-01) is present on the same LAN as web-arm01.
The attacker begins with network and service reconnaissance to identify
reachable hosts and exposed services.

Upon identifying the Apache web server on web-arm01, the attacker performs
HTTP enumeration and basic vulnerability probing.

If misconfigurations or weak controls are identified, the attacker attempts
initial access to the target system.

Following access, the attacker performs limited post-compromise actions
focused on discovery and lateral awareness rather than persistence or impact.

The objective of the operation is not system compromise itself, but the
detection, logging, and analyst investigation of attacker behavior.

PHASE 1 – Reconnaissance
- Host discovery (ARP / ICMP / TCP)
- Port scanning
- Service identification (HTTP)

PHASE 2 – Initial Access Attempt
- Web enumeration
- Misconfiguration probing
- Credential or access weakness testing

PHASE 3 – Post-Access Discovery (If Access Achieved)
- Local system enumeration
- Network awareness checks
- No persistence or destructive actions

DEFENDER GOALS:

- Detect reconnaissance activity
- Correlate scanning and enumeration patterns
- Identify abnormal HTTP access behavior
- Attribute activity to a hostile internal source
- Document findings with evidence

NON-GOALS:

- Malware deployment
- Data exfiltration
- Denial of Service
- Privilege escalation beyond basic enumeration
- Automated response or blocking
