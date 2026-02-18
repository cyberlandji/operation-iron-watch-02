IN-SCOPE SYSTEMS:

- Safeguard Host
  - Analyst workstation
  - Log review and investigation activities
  - SSH access to monitored assets

- soc-core02 (VM)
  - Central monitoring and SIEM component
  - Log ingestion, correlation, and alerting
  - Host-based and network-based telemetry

- web-arm01 (Raspberry Pi 3B+)
  - Apache web server
  - Primary monitored target
  - Source of HTTP, auth, and system logs

- redforge-01 (Kali Linux VM)
  - Simulated adversary system
  - Executes reconnaissance and attack techniques
  - Considered hostile by default

- Local LAN (Bridged Network)
  - Shared network between redforge-01 and web-arm01
  - Enables realistic lateral and internal attack paths

- Host-to-VM communication
  - SSH and management traffic
  - Monitoring traffic flowing into soc-core02
