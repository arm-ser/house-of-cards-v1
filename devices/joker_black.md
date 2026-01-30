# Joker Black (Dell 16 Plus DB16250)

## Notes

- Primary admin workstation laptop
- Ubuntu Desktop 24.04 with latest kernel (6.16.5)
- No production services - used for administration and development
- WireGuard VPN connection (wg0)
- Docker installed but no running containers
- Connected to network via WiFi (172.16.227.167)

## Device Information

**Card Name**: Joker Black
**Hardware**: Dell 16 Plus DB16250
**CPU**: Intel (TBD)
**RAM**: TBD
**Storage**: TBD
**Firmware**: Version 1.1.0 (2025-01-08)

**Management Interface**: SSH (no exposed port, WireGuard VPN only)

## Network Configuration

**Interface**: wlp0s20f3 (WiFi)
**IP Address**: 172.16.227.167/22
**WireGuard**: 10.8.0.95/32
**DNS**: 127.0.0.53 (systemd-resolved)
**VLAN**: None

## Operating System

**OS**: Ubuntu 24.04 LTS
**Kernel**: Linux 6.16.5-061605-generic

## Running Services

### Docker Containers

No containers currently running.

### Exposed Ports

No publicly exposed ports. SSH access via WireGuard VPN only.

**Local Services**:
- 631: CUPS (printing)
- 53: systemd-resolved (DNS)

## Related

- [Device Service Index](device_service_index.md)
- [Hardware Specs](../hardware/device_hardware_index.md)

---

[← Back to Device Index](device_service_index.md)
