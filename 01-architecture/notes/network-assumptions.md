# ============================================================
# Network Assumptions
# Operation Iron Watch 02
# ============================================================
#
# Purpose:
# --------
# This document defines explicit assumptions about the network
# environment in which Operation Iron Watch 02 operates.
#
# These assumptions frame analysis and prevent ambiguity during
# detection and investigation.
#
# ============================================================
# 1. Baseline Network State
# ============================================================
#
# - The network is assumed to be in a clean, non-compromised
#   state prior to attack simulation.
# - No pre-existing malware or persistence mechanisms exist
#   on lab systems.
#
# ============================================================
# 2. Network Scope
# ============================================================
#
# - Only explicitly defined lab systems are in scope.
# - Home network devices (personal laptops, phones, IoT)
#   are out of scope and must not be targeted.
#
# ============================================================
# 3. Attacker Assumptions
# ============================================================
#
# - The attacker is already present inside the LAN.
# - Initial compromise vector is out of scope.
# - Attacker actions are deliberate and time-bounded.
#
# ============================================================
# 4. Traffic Expectations
# ============================================================
#
# - Background traffic is minimal but may exist.
# - Benign traffic is not intentionally generated unless
#   explicitly stated.
#
# ============================================================
# 5. Network Trust Model
# ============================================================
#
# - The LAN is treated as a semi-trusted environment.
# - Internal traffic is not assumed to be benign by default.
#
# ============================================================
# 6. Routing and Segmentation
# ============================================================
#
# - No VLANs or DMZs are assumed in the baseline architecture.
# - All lab systems share a flat LAN.
#
# ============================================================
# 7. Operational Discipline
# ============================================================
#
# - Lab activities are performed during controlled sessions.
# - Network behavior outside these windows is not attributed
#   to the operation.
#
# ============================================================
# Summary
# ============================================================
#
# These assumptions define the analytical context for
# Operation Iron Watch 02 and must be considered when
# interpreting detections and observations.
#
# ============================================================
