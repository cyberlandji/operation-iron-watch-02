# ============================================================
# Operation Iron Watch 02
# Final Architecture Labels
# ============================================================
#
# ============================================================
# Network
# ============================================================
#
# Network Type:
# - Home / Lab LAN (Bridged)
#
# Address Space:
# - Example: 192.168.0.0/24
#
# Security Note:
# - Lab systems share the LAN with home devices
# - Activity is controlled and time-limited
#
# ============================================================
# Router
# ============================================================
#
# Role:
# - Internet gateway
# - DHCP / NAT
#
# ============================================================
# Safeguard Host (Physical)
# ============================================================
#
# Role:
# - Defensive workstation
# - Hosts monitoring VM
#
# Network Interface:
# - Physical NIC (LAN / Bridged)
#
# ============================================================
# soc-core02 (VM)
# ============================================================
#
# OS:
# - Ubuntu Server 22.04 LTS
#
# Role:
# - Monitoring / SIEM / Analysis Node
#
# Functions:
# - Log ingestion
# - Detection analysis
# - Alert review
#
# Network Interfaces:
# - eth0: Bridged (LAN)
# - eth1: NAT (updates only) [optional]
#
# ============================================================
# web-arm01 (Raspberry Pi 3B+)
# ============================================================
#
# OS:
# - Raspberry Pi OS Lite 64-bit
#
# Role:
# - Web server
# - Log and traffic generator
#
# Services:
# - Apache (HTTP)
# - SSH
#
# Telemetry:
# - System logs
# - Application logs
# - Authentication events
#
# Network Interface:
# - eth0: LAN
#
# ============================================================
# Enforcer Host (Physical)
# ============================================================
#
# Role:
# - Attack workstation
# - Hosts attacker VM
#
# Network Interface:
# - Physical NIC (LAN / Bridged)
#
# ============================================================
# redforge-01 (VM)
# ============================================================
#
# OS:
# - Kali Linux
#
# Role:
# - Attack simulation node
#
# Activities:
# - ICMP probing
# - Network scanning
# - Authentication attempts
#
# Network Interfaces:
# - eth0: Bridged (LAN)
# - eth1: NAT (updates only) [optional]
#
# ============================================================
# Traffic & Telemetry Flow
# ============================================================
#
# Attack Traffic:
# - redforge-01  -->  web-arm01
#
# Telemetry / Logs:
# - web-arm01  -->  soc-core02
#
# Analysis:
# - soc-core02 performs detection and investigation
#
# ============================================================
# Architecture Intent
# ============================================================
#
# This architecture intentionally places the attacker inside
# the LAN to simulate:
# - Insider threat
# - Post-compromise activity
# - Rogue internal host
#
# Future iterations may introduce:
# - Network segmentation
# - DMZ
# - VLANs
#
# ============================================================
