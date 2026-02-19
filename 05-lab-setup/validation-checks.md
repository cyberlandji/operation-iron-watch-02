# Validation Checks

CHECKPOINTS:
- Apache logs generated on web-arm01
- Authentication events logged
- Logs reach soc-core02
- Alerts visible (even noisy)
- Analyst can trace full activity path

05-lab-setup contains:
- WHAT is installed
- WHERE it is installed
- WHEN validation happens

NOT:
- Attack execution
- Detection tuning
- Evidence analysis

# ============================================================
# Operation Iron Watch 02
# Phase 3 — Sanity Check & Baseline Observation (DOCUMENTED)
# ============================================================
# Goal:
# - Prove telemetry + visibility end-to-end (web-arm01 -> soc-core02)
# - Capture "normal" behavior baseline BEFORE any redforge activity
#
# What to screenshot (GitHub evidence):
# - soc-core02: agent list shows web-arm01 Active
# - soc-core02: tail -f archives.log showing SSH/sudo/apache entries
# - web-arm01: curl results (200/404), ssh login, sudo usage
# - soc-core02: alerts.json (may be quiet — screenshot anyway)
# ============================================================

set -euo pipefail

echo "[INFO] Phase 3 baseline: run the commands below EXACTLY and take screenshots."

cat <<'EOF'

# ---------------------------
# A) soc-core02 — HEALTH CHECK
# ---------------------------
sudo /var/ossec/bin/agent_control -lc

# Expected:
# ID: 001, Name: web-arm01, IP: <web-arm01-ip>, Active


# -------------------------------------------
# B) soc-core02 — OPEN TWO LIVE TAIL WINDOWS
# -------------------------------------------
# Terminal 1 (raw ingestion / ground truth):
sudo tail -f /var/ossec/logs/archives/archives.log

# Terminal 2 (alerts only / signal):
sudo tail -f /var/ossec/logs/alerts/alerts.json


# ---------------------------------------
# C) web-arm01 — GENERATE "NORMAL" EVENTS
# ---------------------------------------
# 1) SSH login (normal auth noise)
ssh webadmin@web-arm01

# 2) sudo usage (privileged action baseline)
sudo -v
sudo ls -la /root || true

# 3) service activity (systemd baseline)
# If apache2 is installed:
sudo systemctl restart apache2 || true
sudo systemctl status apache2 --no-pager || true

# 4) basic web traffic baseline (HTTP 200/404)
# If Apache is running:
curl -I http://web-arm01/ || true
curl -I http://web-arm01/nonexistent || true

# 5) optional: simple local checks (creates additional normal logs)
id
whoami
uname -a
uptime


# ---------------------------------------------------------
# D) soc-core02 — CONFIRM YOU SAW EVENTS IN archives.log
# ---------------------------------------------------------
# In the archives tail window, you should see entries related to:
# - sshd (login attempts/sessions)
# - sudo (session opened/closed)
# - systemd (service restart)
# - apache2 (access/error entries) if running

# Optional quick grep in a new terminal (does not replace screenshots):
sudo grep -E "web-arm01|sshd|sudo|apache|systemd" /var/ossec/logs/archives/archives.log | tail -n 50


# -------------------------------------------
# E) SOC NOTE (important, document this text)
# -------------------------------------------
# alerts.json may be quiet during baseline.
# That is GOOD. Baseline is about understanding normal activity.
# archives.log is your proof of ingestion and visibility.



# ============================================================
# Iron Watch 02 – Baseline Validation (Revision)
# ============================================================

# Context:
#   An initial baseline validation was performed
#   BEFORE the Wazuh browser dashboard was integrated.

# Limitation:
#   - No visual confirmation of alerts
#   - No dashboard-level evidence
#   - Limited SOC-style validation

# Decision:
#   Perform a second baseline AFTER dashboard integration.

# Baseline Versions:
#   Baseline v0:
#     - Agent installation
#     - Log generation at system level
#     - No dashboard visibility

#   Baseline v1 (authoritative baseline):
#     - Dashboard accessible via browser
#     - Agents visible and healthy
#     - Benign activity observable in UI
#     - Screenshots captured for evidence

# Rule:
#   Baseline v1 replaces v0 as the official SOC baseline
#   for all future attack analysis.

