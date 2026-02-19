################################################################################
# Operation Iron Watch 02
# Phase 2 – Wazuh Agent Connectivity Troubleshooting
# Case: Firewall-Induced Telemetry Loss (Resolved)
################################################################################

# Environment
# ----------
# SOC Manager   : soc-core02
# Endpoint      : web-arm01
# SIEM          : Wazuh 4.x
# Network       : Dual-homed SOC (NAT + LAN/Host-only/Bridged)
# Firewall      : UFW (nftables backend)
# Expected ports: TCP 1514 (data), TCP 1515 (agent registration)

################################################################################
# 1. Symptom Observation
################################################################################

# On SOC manager, agent is registered but never connects
sudo /var/ossec/bin/agent_control -l

# Expected (problematic) output:
# ID: 001, Name: web-arm01, IP: 192.168.0.9, Never connected

# Screenshot:
# - agent_control showing "Never connected"

################################################################################
# 2. Verify Wazuh Manager Health
################################################################################

# Confirm Wazuh manager is running
sudo systemctl status wazuh-manager

# Confirm ports are listening locally
sudo ss -tulpen | grep 1514
sudo ss -tulpen | grep 1515

# Expected:
# LISTEN 0.0.0.0:1514
# LISTEN 0.0.0.0:1515

# Screenshot:
# - ss output showing wazuh-remoted listening

################################################################################
# 3. Test Network Reachability (Endpoint → SOC)
################################################################################

# From web-arm01, test basic reachability
ping -c 2 192.168.0.10

# Expected:
# ICMP replies OK

# Test Wazuh data port
nc -vz 192.168.0.10 1514

# Test Wazuh registration port
nc -vz 192.168.0.10 1515

# Observed result:
# nc: connect to 192.168.0.10 port 1514 (tcp) timed out
# nc: connect to 192.168.0.10 port 1515 (tcp) timed out

# Screenshot:
# - nc timeout results

################################################################################
# 4. Hypothesis
################################################################################

# Network and routing are correct.
# Wazuh services are running.
# ICMP works, TCP does not.

# Hypothesis:
# Local firewall (UFW/nftables) on soc-core02 is blocking telemetry ports.

################################################################################
# 5. Firewall Inspection
################################################################################

# Check UFW status and rules
sudo ufw status numbered

# Observe:
# - SSH (22/tcp) allowed
# - NO rules for 1514/tcp or 1515/tcp

# Inspect nftables counters (optional deep dive)
sudo nft list ruleset

# Screenshot:
# - ufw rules showing missing Wazuh ports
# - nftables counters indicating drops

################################################################################
# 6. Remediation – Allow Required Telemetry Ports
################################################################################

# Allow Wazuh agent data channel
sudo ufw allow 1514/tcp

# Allow Wazuh agent registration channel
sudo ufw allow 1515/tcp

# Reload firewall
sudo ufw reload

# Verify rules
sudo ufw status numbered

# Expected:
# 1514/tcp ALLOW IN Anywhere
# 1515/tcp ALLOW IN Anywhere

# Screenshot:
# - ufw status with ports 1514 and 1515 allowed

################################################################################
# 7. Restart Services
################################################################################

# Restart Wazuh manager
sudo systemctl restart wazuh-manager

# On endpoint, restart agent
sudo systemctl restart wazuh-agent

################################################################################
# 8. Post-Fix Verification
################################################################################

# Re-test connectivity from web-arm01
nc -vz 192.168.0.10 1514
nc -vz 192.168.0.10 1515

# Expected:
# Connection succeeded

# Verify agent status on SOC
sudo /var/ossec/bin/agent_control -l

# Expected (SUCCESS):
# ID: 001, Name: web-arm01, IP: 192.168.0.9, Active

# Screenshot:
# - agent_control showing "Active"

################################################################################
# 9. Conclusion & Lessons Learned
################################################################################

# Root Cause:
# - Over-hardened SOC firewall blocked SIEM telemetry ports.

# Resolution:
# - Explicitly allowed required Wazuh ports (1514/1515 TCP).

# Security Lesson:
# - A hardened SOC must still explicitly allow telemetry ingestion paths.
# - Silence is not safety.
# - Firewall validation is part of SOC onboarding.

################################################################################
# Phase 2 Status
################################################################################

# Phase 2 – Agent Connectivity: LOCKED ✅
# SOC Hardening: VALIDATED ✅
# Ready for Phase 3 – Log Ingestion & Detection Engineering
################################################################################
