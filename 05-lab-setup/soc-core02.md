ROLE:
Central monitoring and detection VM

STEPS:
1. OS update and hardening baseline
2. Install SIEM components (Wazuh / log stack)
3. Configure log ingestion endpoints
4. Validate service health
5. Direct Upgrade of soc-core

[✔] Wazuh Manager installed on soc-core
[✔] Repository: packages.wazuh.com (4.x stable)
[✔] Service enabled at boot
[✔] Core modules running
[✔] Ready to accept agents

## Wazuh Control Plane Validation

- Wazuh Manager installed and running on soc-core
- wazuh-remoted active and ready for agent communication
- Agent registration port (1515/tcp) listening
- Agent data port (1514) configured and available on demand
- Control plane validated prior to endpoint onboarding
