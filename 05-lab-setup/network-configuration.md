# Network Configuration

NETWORK MODEL:
- Bridged networking for redforge-01
- Ethernet LAN for web-arm01
- Internal LAN visibility

IP ASSUMPTIONS:
- web-arm01: DHCP (stable lease)
- soc-core02: NAT + Host-only
- redforge-01: Bridged LAN IP

VALIDATION:
- All hosts reachable where intended
- soc-core02 can receive logs from web-arm01
