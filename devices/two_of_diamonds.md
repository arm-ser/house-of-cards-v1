# Two of Diamonds (NAS Server - Proxmox Host)

## Notes

- Physical server hosting Proxmox VE virtualization platform
- Runs multiple VMs including TrueNAS (King of Diamonds), DNS (Six of Diamonds), and Matrix server
- Dual network bridges for Main LAN and DMZ segmentation
- Heavy storage configuration with 3x 8TB HDDs + 2x 500GB SSDs + 256GB NVMe boot
- Power consumption varies by VM load (100W-300W typical)
- Local network only - no external exposure

## Device Information

**Card Name**: Two of Diamonds
**Hardware**: Custom NAS Server Build
**CPU**: Intel Core i7-8700K @ 3.70GHz (8 cores, 1 socket)
**RAM**: 32GB DDR4 2666MHz
**Storage**:
- 256GB M.2 NVMe - Proxmox boot
- 2x 500GB SSD
- 3x 8TB HDD - Passed through to TrueNAS
**Power Draw**: 100W-300W
**Network**: 2x 2.5GbE interfaces

**Management Interface**: Proxmox Web UI (port 8006), SSH (port 22)

## Operating System

**OS**: Proxmox VE (Debian-based)

## Network Configuration

**IP Address (vmbr0)**: 192.168.1.2 (Main LAN)
**IP Address (vmbr1)**: 10.0.1.2 (DMZ)
**Hostname**: two.dmnd
**DNS**: 192.168.1.50

## Hosted Virtual Machines

### VM 100 (King of Diamonds)

**Purpose**: TrueNAS SCALE storage server
**Resources**: 16GB RAM, 128GB disk
**Network**: vmbr0 (192.168.1.62)

### VM 101 (Seven of Spades)

**Purpose**: Immich photo server
**Resources**: 10GB RAM, 32GB disk
**Network**: vmbr0 (192.168.1.107)

### VM 103 (Six of Diamonds)

**Purpose**: Secondary DNS server with PiHole
**Resources**: 1GB RAM, 10GB disk
**Network**: vmbr0 (192.168.1.60)

### VM 105 (Matrix Server)

**Purpose**: Matrix chat server in DMZ
**Resources**: 4GB RAM, 32GB disk
**Network**: vmbr1 (10.0.1.120)

## Storage Configuration

### Local Storage (Proxmox)

| Device       | Type     | Capacity | Usage                 |
| ------------ | -------- | -------- | --------------------- |
| /dev/nvme0n1 | NVMe SSD | 256GB    | Proxmox boot & system |

### Passthrough Storage (TrueNAS)

| Device   | Type | Capacity | Passed to        |
| -------- | ---- | -------- | ---------------- |
| /dev/sda | SSD  | 1TB      | VM 100 (TrueNAS) |
| /dev/sdb | SSD  | 1TB      | VM 100 (TrueNAS) |
| /dev/sdc | HDD  | 8TB      | VM 100 (TrueNAS) |
| /dev/sdd | SSD  | 500GB    | VM 100 (TrueNAS) |
| /dev/sde | SSD  | 500GB    | VM 100 (TrueNAS) |
| /dev/sdf | HDD  | 8TB      | VM 100 (TrueNAS) |
| /dev/sdg | HDD  | 8TB      | VM 100 (TrueNAS) |

**Disk Mapping to Pools**:
- **media pool**: /dev/sdc, /dev/sdf, /dev/sdg (3x 8TB HDD)
- **files pool**: /dev/sdd, /dev/sde (2x 500GB SSD)
- **gallery pool**: /dev/sda, /dev/sdb (2x 1TB SSD)

**Note**: All disks are passed through to TrueNAS VM for ZFS pool management.

## Exposed Ports

| Port | Protocol | Service | External Access |
|------|----------|---------|-----------------|
| 22 | TCP | SSH | Local only |
| 8006 | TCP | Proxmox Web UI | Local only |

## Related

- [Device Service Index](device_service_index.md)
- [Hardware Specs](../hardware/device_hardware_index.md)

---

[← Back to Device Index](device_service_index.md)
