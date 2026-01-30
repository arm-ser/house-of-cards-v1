# King of Diamonds (TrueNAS SCALE Server)

## Notes

- VM running on Two of Diamonds (Proxmox host)
- Primary network storage server for the homelab
- Provides NFS/SMB shares for all devices on Main LAN
- Uses passthrough disks from Proxmox host for direct hardware access
- ZFS storage pools for data integrity and snapshots
- Local network only - no external exposure

## Device Information

**Card Name**: King of Diamonds
**Hardware**: VM on Proxmox (Two of Diamonds)
**Host**: Two of Diamonds (VM 100)
**CPU**: 4 vCPU
**RAM**: 16GB
**Storage**: 128GB boot + passthrough disks (3x 8TB HDD, 2x 500GB SSD, 2x 1TB SSD)
**Power Draw**: ~60W
**Network**: VirtIO 1GbE

**Management Interface**: TrueNAS Web UI (HTTPS port 443), SSH (port 22)

## Network Configuration

**IP Address**: 192.168.1.62
**Hostname**: king.dmnd
**DNS**: 192.168.1.50

## Operating System

**OS**: TrueNAS SCALE (Debian-based)

## Storage Configuration

### Boot Disk

| Device | Type | Capacity | Purpose |
|--------|------|----------|---------|
| scsi0 | Virtual (qcow2) | 128GB | TrueNAS SCALE OS |

### Passthrough Disks

| SCSI ID | Type | Capacity | Pool Assignment |
|---------|------|----------|-----------------|
| scsi5 | HDD | 8TB | media |
| scsi7 | HDD | 8TB | media |
| scsi10 | HDD | 8TB | media |
| scsi20 | SSD | 500GB | files |
| scsi21 | SSD | 500GB | files |
| scsi22 | SSD | 1TB | gallery |
| scsi23 | SSD | 1TB | gallery |

### ZFS Storage Pools

#### Pool: boot-pool

**Capacity**: 111GB
**Used**: 10%
**Purpose**: TrueNAS SCALE OS boot pool

#### Pool: files

**Configuration**: 2x 500GB SSD (mirror)
**Capacity**: 464GB
**Used**: 1%
**Purpose**: Fast storage for Docker volumes and frequently accessed data
**Mountpoint**: /mnt/files
**Disks**: scsi20, scsi21

#### Pool: gallery

**Configuration**: 2x 1TB SSD (mirror)
**Capacity**: 928GB
**Used**: 3%
**Purpose**: Photo and gallery storage
**Mountpoint**: /mnt/gallery
**Disks**: scsi22, scsi23

#### Pool: media

**Configuration**: 3x 8TB HDD (RAIDZ1)
**Capacity**: 21.8TB
**Used**: 10%
**Purpose**: Bulk media storage (movies, music, podcasts, videos, backups)
**Mountpoint**: /mnt/media
**Disks**: scsi5, scsi7, scsi10

## Running Services

### TrueNAS Services

| Service Name | Purpose | Protocol | Status |
| ------------ | ------- | -------- | ------ |
| SMB | Windows file sharing | TCP 445 | TBD |
| NFS | Unix file sharing | TCP 2049 | TBD |
| SSH | Remote management | TCP 22 | TBD |
| WebUI | Web management interface | HTTPS 443 | Active |

**Note**: Document active services and shares after reviewing TrueNAS configuration

### Network Shares (SMB)

| Share Name | Path                  | Purpose                        |
| ---------- | --------------------- | ------------------------------ |
| docker     | /mnt/backup/docker    | Docker configs and data backup |
| data       | /mnt/files/data       | General data storage           |
| format     | /mnt/files/format     | Format/template storage        |
| portable   | /mnt/files/portable   | Portable files                 |
| gallery    | /mnt/media/collection | Photo gallery                  |
| backup     | /mnt/media/backup     | General backup storage         |
| collection | /mnt/media/collection | Media collection               |

**TrueNAS Apps**: TrueNAS SCALE includes Kubernetes (k3s) for app deployment with ix-applications dataset.

## Exposed Ports

| Port | Protocol | Service | External Access |
|------|----------|---------|-----------------|
| 22 | TCP | SSH | Local only |
| 445 | TCP | SMB | Local only |
| 2049 | TCP | NFS | Local only |
| 443 | TCP | TrueNAS Web UI | Local only |

## Related

- [Device Service Index](device_service_index.md)
- [Hardware Specs](../hardware/device_hardware_index.md)

---

[← Back to Device Index](device_service_index.md)
