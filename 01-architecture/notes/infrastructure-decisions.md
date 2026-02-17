# ============================================================
# Infrastructure Decisions
# Operation Iron Watch 02
# ============================================================
#
# Purpose:
# --------
# This document explains WHY specific infrastructure choices
# were made for Operation Iron Watch 02.
#
# It complements the README Architecture section and does NOT
# redefine hosts, IP addresses, or topology.
#
# ============================================================
# 1. Design Philosophy
# ============================================================
#
# - Prioritize clarity over complexity
# - Build understanding before realism
# - Favor observable behavior over hidden mechanics
#
# Rationale:
# Early-stage SOC practice benefits more from controlled and
# understandable environments than from fully realistic but
# opaque setups.
#
# ============================================================
# 2. Choice of Bridged Networking
# ============================================================
#
# Decision:
# ---------
# Virtual machines are connected using bridged networking,
# placing them on the same LAN as physical lab devices.
#
# Rationale:
# ----------
# - Simulates an internal LAN attacker scenario
# - Reflects common real-world incidents involving:
#   * compromised internal hosts
#   * rogue devices
#   * insider threats
# - Avoids artificial routing paths that hide traffic
#
# Trade-offs:
# -----------
# - Reduced isolation from home network
# - Requires careful operational discipline
#
# Mitigation:
# -----------
# - Lab activity is time-bounded
# - Targets are known and controlled
# - Non-lab devices are out of scope
#
# ============================================================
# 3. Attacker Placement Inside the LAN
# ============================================================
#
# Decision:
# ---------
# The attack simulation node (redforge-01) operates from
# within the same LAN as the target systems.
#
# Rationale:
# ----------
# - Many SOC investigations begin with internal traffic
# - Post-compromise and insider scenarios are realistic
# - Emphasizes detection of behavior, not perimeter bypass
#
# Assumption:
# -----------
# Initial compromise vector is out of scope for this operation.
#
# ============================================================
# 4. Separation of Roles Across Hosts
# ============================================================
#
# Decision:
# ---------
# Attacker, target, and monitoring roles are separated across
# distinct machines or virtual machines.
#
# Rationale:
# ----------
# - Prevents role ambiguity
# - Clarifies traffic direction and intent
# - Aligns with SOC mental models (source, target, observer)
#
# ============================================================
# 5. Monitoring-Centric Architecture
# ============================================================
#
# Decision:
# ---------
# The architecture is designed around observation and analysis
# rather than prevention or containment.
#
# Rationale:
# ----------
# - The objective is SOC detection and investigation
# - Response actions are deferred to later operations
# - Focus remains on visibility and understanding
#
# ============================================================
# 6. Deferred Use of Network Segmentation
# ============================================================
#
# Decision:
# ---------
# DMZs, VLANs, and advanced segmentation are not implemented
# in the initial Iron Watch 02 architecture.
#
# Rationale:
# ----------
# - Adds complexity without immediate analytical benefit
# - Better introduced after baseline behavior is understood
#
# Future Plan:
# ------------
# Segmentation will be introduced in later iterations once
# detection fundamentals are solid.
#
# ============================================================
# 7. Hardware and Environmental Constraints
# ============================================================
#
# Considerations:
# ---------------
# - Home lab environment
# - Single consumer-grade router
# - Limited managed switching capabilities
#
# Rationale:
# ----------
# Architecture choices reflect realistic constraints faced
# by many junior analysts and home labs.
#
# ============================================================
# 8. Documentation-First Approach
# ============================================================
#
# Decision:
# ---------
# Infrastructure decisions are explicitly documented before
# execution.
#
# Rationale:
# ----------
# - Reduces ambiguity
# - Supports future review and iteration
# - Mirrors professional engineering and SOC practices
#
# ============================================================
# Summary
# ============================================================
#
# The Iron Watch 02 infrastructure is intentionally simple,
# explicit, and internally consistent.
#
# Each decision prioritizes learning value, observability,
# and analytical clarity over maximum realism.
#
# ============================================================
