###############################################################################
# FILE: 09-postmortems/soc-core02-postmortem.md
# CONTEXT: Operation Iron Watch 02
# PURPOSE: Technical postmortem (GitHub-facing)
###############################################################################

cat << 'EOF' > soc-core02-postmortem.md
# soc-core02 Postmortem

## Summary
soc-core02 was retired after an in-place overwrite of the Wazuh stack caused
service corruption and system instability. Recovery was deemed non-viable,
and a clean rebuild (soc-core03) was initiated.

This postmortem documents the incident and the architectural decisions that
followed.

---

## System Role
- **Host:** soc-core02
- **Purpose:** SOC core node (SIEM, detection, log analysis)
- **Primary Tooling:** Wazuh (manager, indexer, dashboard)

---

## What Happened
- An in-place overwrite of Wazuh components was attempted
- Core services failed to restart or disappeared
- System became unstable and unusable
- No clean rollback path was available

---

## Root Causes (High-Level)
- Overwriting a complex security stack in-place
- Insufficient isolation between experimentation and baseline system
- No immediate snapshot taken prior to overwrite

---

## Impact
- soc-core02 declared unrecoverable
- Iron Watch 02 temporarily frozen
- Decision taken to rebuild from scratch

---

## Corrective Actions
- New VM created: **soc-core03**
- Clean installation approach adopted
- Graylog-first detection strategy chosen
- Stronger emphasis on snapshots and rollback planning

---

## Lessons Learned
- Overwriting ≠ upgrading
- Snapshots are mandatory before major changes
- Simpler tooling accelerates learning at early stages

---
