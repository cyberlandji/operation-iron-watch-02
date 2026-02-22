# STEP 1 — SOC Core Baseline Validation (soc-core03)

## Purpose

Before ingesting logs or simulating any threat activity, the SOC platform **must be trustworthy**.
This step validates that `soc-core03` is stable, reachable, predictable, and suitable as a detection foundation.

This is **not hardening** and **not detection engineering**.
This is a **baseline sanity check**.

---

## Scope

- Host: soc-core03
- Role: Central SOC / log analysis node
- Operation: Iron Watch 02
- Phase: Pre-ingestion
- State: Clean rebuild after soc-core02 decommission

---

## 1. System Identity & OS Health

### Why this matters

Operators must always know exactly **which system** they are working on.
Unexpected OS state or instability invalidates all downstream work.

### Commands

```bash
hostnamectl
lsb_release -a || cat /etc/os-release
uptime
```

### Expected

- Hostname is set to `soc-core03`
- OS version is supported and known
- No abnormal load or repeated uptime resets

📸 Evidence:

- Hostname
- OS version

---

## 2. Disk & Filesystem Health

### Why this matters

SIEM platforms are disk-intensive.

Filesystem instability leads to silent data loss and broken detections.

### Commands

```
df-h
lsblk
sudo mount | column-t
```

Optional read-only filesystem state check:

```
sudo tune2fs-l $(df / | tail -1 | awk'{print $1}') |grep-i'Filesystem state'
```

### Expected

- Sufficient free disk space
- Root filesystem mounted read-write
- No filesystem errors reported

📸 Evidence:

- Disk usage overview

---

## 3. Memory & CPU Sanity

### Why this matters

Resource starvation causes delayed ingestion, missed alerts, and service crashes.

### Commands

```
free-h
top-o %CPU
nproc
```

### Expected

- Available free memory
- CPU not permanently saturated
- Core count appropriate for lab usage

📸 Evidence:

- Memory and CPU snapshot

---

## 4. Network Configuration & Connectivity

### Why this matters

SOC visibility depends on **stable and predictable networking**.

### Commands

```
ip a
ip r
```

DNS verification:

```
resolvectl status ||cat /etc/resolv.conf
```

External connectivity test (if intended):

```
ping-c38.8.8.8
```

### Expected

- Network interfaces up and documented
- Default route present
- DNS resolution functional
- External connectivity confirmed (if designed)

📸 Evidence:

- IP addresses
- Routing table

---

## 5. SSH Stability & Access

### Why this matters

SOC nodes must remain reachable during investigations and incidents.

### Commands

```
systemctl statusssh
ss-tulpn |grepssh
```

Optional log inspection:

```
sudo journalctl-ussh--no-pager | tail
```

### Expected

- SSH service active
- Port listening as intended
- No repeated crashes or authentication errors

📸 Evidence:

- SSH service status

---

## 6. Time Synchronization (Critical)

### Why this matters

Incorrect timestamps break correlation, timelines, and investigations.

### Commands

```
timedatectl
```

Optional:

```
timedatectl timesync-status
```

### Expected

- NTP enabled
- System clock synchronized
- Correct timezone configured

📸 Evidence:

- timedatectl output

---

## 7. Firewall State (Intentional, Not Accidental)

### Why this matters

A SOC must be protected, but **never unintentionally blinded**.

### Commands

```
sudo ufw status verbose ||sudo iptables-L-n
```

### Expected

- Firewall state clearly understood
- No unknown blocking rules
- SSH explicitly allowed if firewall is enabled

📸 Evidence:

- Firewall rules snapshot

---

## 8. Running Services Baseline (No SIEM Yet)

### Why this matters

At this stage, no SOC or SIEM services should be running.

This ensures a clean and controlled starting point.

### Commands

```
systemctl list-units--type=service--state=running
```

### Expected

- Only OS-level services running
- No Wazuh, Elastic, Graylog, or residual SOC components
- Clean baseline confirmed

📸 Evidence:

- Running services list

---

## 9. Snapshot Creation (Mandatory)

### Why this matters

Snapshots are part of SOC operational discipline.

They are not optional.

### Action

Create a VM snapshot **now**.

Suggested name:

```
soc-core03_baseline_clean
```

📸 Evidence:

- Snapshot created (hypervisor-side screenshot)

---

## Baseline Status

If all checks above are satisfied:

- soc-core03 is stable
- Platform is trustworthy
- SOC work may proceed
