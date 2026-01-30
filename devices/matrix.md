# Matrix (Matrix Homeserver)

## Notes

- VM running on Two of Diamonds (Proxmox host)
- Matrix protocol homeserver for secure, decentralized communication
- Isolated in DMZ network for security separation from main LAN
- Local DMZ access only - no external exposure without VPN

## Device Information

**Card Name**: Matrix
**Hardware**: VM on Proxmox (Two of Diamonds)
**Host**: Two of Diamonds (VM 105)
**CPU**: 4 vCPU
**RAM**: 4GB
**Storage**: 32GB
**Power Draw**: ~10W
**Network**: VirtIO 1GbE

**Management Interface**: SSH (port 22)

## Network Configuration

**IP Address**: 10.0.1.120
**Hostname**: matrix
**DNS**: 192.168.1.50

## Operating System

**OS**: Debian GNU/Linux 12

## Running Services

### Docker Containers

| Container Name     | Purpose                       | Ports |
| ------------------ | ----------------------------- | ----- |
| matrix-bot         | Matrix bot (Python)           | -     |
| synapse            | Matrix homeserver (Synapse)   | 9001  |
| cloudflared-matrix | Cloudflare tunnel for Matrix  | -     |
| portainer          | Container management          | 9002  |

### Exposed Ports

| Port | Service           |
| ---- | ----------------- |
| 22   | SSH               |
| 9001 | Matrix Client API |
| 9002 | Portainer HTTPS   |

## Related

- [Device Service Index](device_service_index.md)
- [Hardware Specs](../hardware/device_hardware_index.md)

---

[← Back to Device Index](device_service_index.md)