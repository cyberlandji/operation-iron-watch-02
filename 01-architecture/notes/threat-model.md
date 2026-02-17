# ============================================================
# Threat Model
# Operation Iron Watch 02
# ============================================================
#
# Purpose:
# --------
# This document defines the threat model used for
# Operation Iron Watch 02.
#
# It specifies the type of attacker being simulated,
# their capabilities, limitations, and intent.
#
# ============================================================
# 1. Threat Actor Profile
# ============================================================
#
# Actor Type:
# -----------
# - Internal attacker
# - Post-compromise or rogue internal host
#
# Examples:
# ---------
# - Compromised workstation
# - Malicious insider
# - Unauthorized device connected to LAN
#
# ============================================================
# 2. Attacker Skill Level
# ============================================================
#
# Skill Level:
# ------------
# - Low to moderate sophistication
#
# Characteristics:
# ---------------
# - Uses common tools and techniques
# - Relies on default configurations
# - Limited stealth and evasion awareness
#
# ============================================================
# 3. Attacker Capabilities
# ============================================================
#
# Allowed Capabilities:
# ---------------------
# - Network reconnaissance
# - Service enumeration
# - Authentication attempts
# - Basic exploitation where applicable
#
# ============================================================
# 4. Attacker Limitations
# ============================================================
#
# Explicit Limitations:
# ---------------------
# - No zero-day exploits
# - No kernel-level exploitation
# - No firmware or hardware attacks
# - No advanced persistence mechanisms
#
# Rationale:
# ----------
# Focus is on observable and common SOC scenarios,
# not nation-state or highly advanced threats.
#
# ============================================================
# 5. Initial Access Assumptions
# ============================================================
#
# Assumption:
# -----------
# The attacker is already present inside the LAN.
#
# Out of Scope:
# -------------
# - Phishing
# - Supply-chain compromise
# - Physical intrusion
#
# ============================================================
# 6. Objectives of the Attacker
# ============================================================
#
# Primary Objectives:
# -------------------
# - Discover live hosts
# - Identify exposed services
# - Attempt access to available services
#
# Secondary Objectives:
# ---------------------
# - None by default
#
# ============================================================
# 7. Target Selection
# ============================================================
#
# Target Characteristics:
# -----------------------
# - Known lab systems only
# - No interaction with non-lab devices
#
# Rationale:
# ----------
# Ensures controlled experimentation and safety.
#
# ============================================================
# 8. Expected Observable Behaviors
# ============================================================
#
# Expected Signals:
# -----------------
# - ICMP probing
# - TCP connection attempts
# - Authentication failures
# - Application-level requests
#
# ============================================================
# 9. Threat Model Boundaries
# ============================================================
#
# The following are explicitly NOT part of this threat model:
# ----------------------------------------------------------
# - Advanced APT tradecraft
# - Long-term persistence
# - Stealthy command-and-control
# - Data exfiltration campaigns
#
# ============================================================
# 10. Evolution Path
# ============================================================
#
# This threat model may evolve in future operations to include:
# -------------------------------------------------------------
# - Higher attacker sophistication
# - Persistence mechanisms
# - Multi-stage attack chains
#
# ============================================================
# Summary
# ============================================================
#
# The Iron Watch 02 threat model focuses on realistic,
# observable internal attacker behavior commonly encountered
# by SOC teams, emphasizing detection and investigation over
# advanced exploitation.
#
# ============================================================
