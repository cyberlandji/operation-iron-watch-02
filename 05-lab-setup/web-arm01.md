ROLE:
- Monitored target system
- Apache web server

STEPS:
1. Update Raspberry Pi OS Lite
2. Install and start Apache
3. Enable and verify logging
4. Prepare SSH access
5. Confirm log generation

################################################################################
# web-arm01 Setup – Operation Iron Watch 02
# Role: Monitored Web Target
# OS: Raspberry Pi OS Lite (64-bit, systemd)
# Services: Apache2, OpenSSH
# Logging: Apache (file-based) + SSH/System (journald)
################################################################################

############################
# Step 1 – System Baseline
############################

sudo apt update && sudo apt upgrade -y

hostnamectl
ip a
ip route

############################
# Step 2 – Apache2 Install
############################

sudo apt install apache2 -y
sudo systemctl enable apache2
sudo systemctl start apache2
sudo systemctl status apache2

############################
# Step 3 – Local HTTP Test
############################

curl http://localhost

############################
# Step 4 – Listening Port Verification
############################

sudo ss -tulnp | grep :80

############################
# Step 5 – Apache Logging Validation
############################

# List Apache log directory
ls -lah /var/log/apache2/

# Generate HTTP traffic FROM ANOTHER HOST (soc-core02 or host machine)
# curl http://<web-arm01-ip>/
# curl http://<web-arm01-ip>/this-page-does-not-exist

# Verify access logs
sudo tail -n 20 /var/log/apache2/access.log

# Optional: check error log
sudo tail -n 20 /var/log/apache2/error.log

############################
# Step 6 – SSH Service Check
############################

sudo systemctl status ssh

############################
# Step 7 – SSH Authentication Logging (journald)
############################

# Generate SSH activity FROM soc-core02
# ssh webadmin@<web-arm01-ip>
# exit

# Verify SSH logs (Raspberry Pi OS uses journald)
sudo journalctl -u ssh --no-pager | tail -n 50

############################
# Step 8 – System Logging Confirmation
############################

sudo journalctl --disk-usage

############################
# Notes (Important)
############################

# - /var/log/auth.log does NOT exist on Raspberry Pi OS by default
# - /var/log/syslog does NOT exist by default
# - SSH and system events are logged via systemd journal
# - Apache HTTP logs are stored in /var/log/apache2/

############################
# Explicit Non-Actions
############################

# DO NOT harden Apache
# DO NOT modify SSH configuration
# DO NOT enable firewall rules
# DO NOT install SIEM agents yet
# DO NOT force auth.log or syslog creation

############################
# Validation Checklist
############################

# [✓] Apache running
# [✓] Port 80 listening
# [✓] HTTP 200 logged
# [✓] HTTP 404 logged
# [✓] SSH auth events visible via journalctl
# [✓] System logs available

############################
# Status
############################

# web-arm01: READY FOR CENTRAL LOG INGESTION
################################################################################
