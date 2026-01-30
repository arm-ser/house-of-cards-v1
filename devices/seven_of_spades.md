# Seven of Spades (Proxmox VM)

## Notes

- Immich photo server for personal photo library management
- Runs on Proxmox VM (two-of-diamonds host)
- Machine learning features for facial recognition and object detection
- All services local-only, no internet exposure
- Standard docker data directory: /home/homelab/docker/[service-name]

## Device Information

**Card Name**: Seven of Spades
**Hardware**: Proxmox VM on two-of-diamonds
**Virtualization**: KVM (QEMU)
**CPU**: Allocated vCPUs
**RAM**: 10GB
**Storage**: 32GB disk

**Management Interface**: SSH (port 22)

## Network Configuration

**IP Address**: 192.168.1.107
**Hostname**: seven.spds
**DNS**: 192.168.1.50

## Operating System

**OS**: Ubuntu 24.04.3 LTS

## Running Services

### Docker Containers

| Container Name          | Purpose                           | Ports |
| ----------------------- | --------------------------------- | ----- |
| immich_server           | Immich photo server               | 9001  |
| immich_machine_learning | ML service for photo analysis     | -     |
| immich_db               | PostgreSQL with vector extensions | -     |
| immich_redis            | Redis cache for Immich            | -     |
| portainer               | Container management              | 9002  |

### Exposed Ports

| Port | Service         |
| ---- | --------------- |
| 22   | SSH             |
| 9001 | Immich Web UI   |
| 9002 | Portainer HTTPS |

## Related

- [Device Service Index](device_service_index.md)
- [Hardware Specs](../hardware/device_hardware_index.md)

---

[← Back to Device Index](device_service_index.md)
