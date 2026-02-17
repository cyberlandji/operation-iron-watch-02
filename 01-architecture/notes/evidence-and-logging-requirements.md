# ============================================================
# Evidence & Logging Requirements
# Operation Iron Watch 02
# ============================================================
#
# Purpose:
# --------
# This document defines the minimum evidence and logging
# requirements for Operation Iron Watch 02.
#
# It exists to ensure that analytical conclusions are supported
# by retained artifacts and not only by live observation.
#
# ============================================================
# 1. Guiding Principle
# ============================================================
#
# Evidence must be:
# - Observable
# - Retained
# - Correlatable
#
# Live observation alone is insufficient for post-analysis.
#
# ============================================================
# 2. Evidence Categories
# ============================================================
#
# Evidence collected during IW-02 is grouped into:
#
# - Network-based evidence
# - Host-based evidence
# - Attacker activity evidence
# - Analyst-generated notes
#
# ============================================================
# 3. Network-Based Evidence Requirements
# ============================================================
#
# Required:
# ---------
# - Network detection alerts (e.g. IDS/IPS output)
# - Timestamps associated with alerts
#
# Optional (if available):
# ------------------------
# - Packet-level captures for specific events
#
# Rationale:
# ----------
# Network telemetry provides attack sequencing and timing.
#
# ============================================================
# 4. Host-Based Evidence Requirements
# ============================================================
#
# Target Systems:
# ---------------
# - Authentication logs
# - System logs
# - Application logs (where applicable)
#
# Minimum Requirement:
# --------------------
# - Logs must be preserved for the duration of the operation
#
# Rationale:
# ----------
# Host logs provide context and attribution that network-only
# telemetry cannot fully supply.
#
# ============================================================
# 5. Attacker Activity Evidence
# ============================================================
#
# Required:
# ---------
# - Commands executed by the attacker
# - Tool usage and parameters
#
# Acceptable Forms:
# -----------------
# - Saved command output
# - Shell history snapshots
# - Manual command transcripts
#
# Rationale:
# ----------
# Attacker-side evidence supports correlation and intent analysis.
#
# ============================================================
# 6. Time Synchronization Requirement
# ============================================================
#
# Requirement:
# ------------
# - All lab systems must use consistent time sources
#
# Rationale:
# ----------
# Correlation depends on accurate temporal alignment between
# attacker actions and observed events.
#
# ============================================================
# 7. Evidence Storage Structure
# ============================================================
#
# Evidence must be stored in a structured and predictable layout.
#
# Example:
# --------
# evidence/
# ├── network/
# ├── host/
# ├── attacker/
# └── analyst-notes/
#
# Rationale:
# ----------
# Consistent structure enables repeatability and review.
#
# ============================================================
# 8. Evidence Integrity Expectations
# ============================================================
#
# - Evidence is collected without modification
# - No retroactive alteration of artifacts
# - Replays must be explicitly labeled as replays
#
# Rationale:
# ----------
# Integrity is critical to analytical credibility.
#
# ============================================================
# 9. Known Limitations
# ============================================================
#
# Accepted Constraints:
# ---------------------
# - Full packet capture may not always be available
# - Some ephemeral events may not be logged
#
# These limitations must be documented when encountered.
#
# ============================================================
# 10. Analyst Responsibilities
# ============================================================
#
# The analyst is responsible for:
# - Verifying evidence collection before attacks
# - Noting gaps during analysis
# - Documenting assumptions and limitations
#
# ============================================================
# Summary
# ============================================================
#
# Operation Iron Watch 02 treats evidence collection as a
# first-class objective.
#
# Detection without retained evidence is considered incomplete.
#
# ============================================================
