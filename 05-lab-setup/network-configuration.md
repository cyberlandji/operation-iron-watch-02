# Network Configuration

NETWORK MODEL:
- Bridged networking for redforge-01
- Bridged networking for soc-core03
- Ethernet LAN for web-arm01
- Internal LAN visibility

IP ASSUMPTIONS:
- web-arm01: DHCP (stable lease)
- soc-core03: NAT + Host-only + Bridged
- redforge-01: NAT + Bridged LAN IP

VALIDATION:
- All hosts reachable where intended
- soc-core03 can receive logs from web-arm01
