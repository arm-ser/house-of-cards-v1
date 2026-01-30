# Homelab Infrastructure Guide

Obsidian vault for documenting and managing home lab infrastructure.

## Project Phases

1. **Documentation**: Inventory devices, services, configurations
2. **Optimization**: Consolidate services, improve performance
3. **Security & Organization**: Implement best practices, segmentation, standardization

## Hardware Summary

See [device_hardware_index.md](../hardware/device_hardware_index.md) for full inventory.

**Mini PCs**: VNOPN (N3700, 8GB, 4x2.5GbE), MeLE (N5105, 8GB), CTONE (N95, 16GB), MINISFORUM GK41 (J4125, 8GB), GMKtec (N4100, 6GB)

**Network**: TP-Link switches (16/8/5-port), Netgear R7900P (AC3000), Netgear AX1800 (WiFi 6), Cable modem

**Servers**: NAS (i7-8700K, 32GB, 48TB HDD+2TB SSD), AI (i7-8700K, 64GB, RTX 3090), Gaming (i7-9700K, 32GB, RTX 5070)

**Clients**: Dell Inspiron 15 (pentest), HP EliteBook 845 G7 (admin)

## Documentation Standards

See `CLAUDE.md` for all formatting, naming, and organizational conventions.

**Media files**: Store in `/attachments/` and reference using `![](<../attachments/file.png>)`

**Hardware tables**: Include columns for Picture, Device, Specs, Power (W), Amazon Link

## Directory Structure

```
house-of-cards-v2/
├── hardware/          # Hardware inventory and device naming
├── devices/           # Per-device service documentation
├── network/           # Network topology and configuration
├── documentation/     # Guides and reference materials
├── diagrams/          # Network diagrams and visual documentation
└── attachments/       # Device photos and screenshots
```

## Workflow Templates

**New Hardware**:
1. Process image → `/attachments/`
2. Add to inventory table with specs + power + link

**Service Documentation**:
1. Current state (what, where, how)
2. Dependencies
3. Network requirements (ports, protocols, VLANs)
4. Data paths and storage

**Infrastructure Changes**:
1. Document current state
2. Identify issues/inefficiencies
3. Research best practices
4. Create plan with rollback procedure
5. Update docs after changes

## Key Constraints

- **Power**: 4W (switches) to 600W (AI/gaming peak)
- **Network**: Mixed 1GbE/2.5GbE, VNOPN has 4x2.5GbE for routing
- **Storage**: 48TB HDD + 2TB SSD on NAS, plan for RAID/backup
- **Compute**: Mini PCs for clustering, dedicated servers for AI/storage

## Best Practices

- **Segmentation**: VLANs for management/services/guest/IoT
- **Documentation First**: Document before changes
- **Power Efficiency**: Optimize for 24/7, consolidate services
- **Backup**: 3-2-1 rule (3 copies, 2 media, 1 offsite)
- **Security**: Firewall rules, isolation, updates, monitoring
- **Labeling**: Physical labels match documentation

## Git Commits

Standard format: `vault backup: YYYY-MM-DD HH:MM:SS`

For significant changes: descriptive messages explaining modifications.
