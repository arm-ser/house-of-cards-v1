# Quick Reference - AI Agent Cheat Sheet

Single-page reference for understanding the entire homelab infrastructure.

## Network Quick Facts

| Network   | Subnet         | Gateway     | Purpose                        |
| --------- | -------------- | ----------- | ------------------------------ |
| LAN       | 192.168.1.0/24 | 192.168.1.1 | Servers & infrastructure       |
| GUST      | 172.16.1.0/24  | 172.16.1.1  | WiFi & workstations            |
| DMZ       | 10.0.1.0/24    | 10.0.1.10   | Isolated services (Cloudflare) |
| WireGuard | 10.10.5.1/24   | 10.10.5.1   | VPN remote access              |

## All Devices Quick Lookup

### Infrastructure (LAN)
- `192.168.1.1` - wall-of-cards (pfSense firewall, 4x 2.5GbE)
- `192.168.1.2` - two-of-diamonds (Proxmox host, i7-8700K, 32GB)
- `192.168.1.50` - jack-of-diamonds (MeLE Mini PC, Primary DNS/Services)
- `192.168.1.62` - king-of-diamonds (TrueNAS VM, 48TB ZFS storage)
- `192.168.1.100` - six-of-spades (CTONE Mini PC, multi-purpose)
- `192.168.1.107` - seven-of-spades (Immich VM, photo server)
- `192.168.1.200` - three-of-diamonds (AI server, i7-8700K, RTX 3090, 64GB)
- `192.168.1.250` - queen-of-diamonds (GMKtec Mini PC, media automation)

### Workstations & WiFi (GUST)
- `172.16.1.5` - joker-black (Dell 16 Plus, admin workstation)
- `172.16.1.10` - ten-of-spades (Gaming PC, also on LAN .10)
- `172.16.1.15` - guest-accesspoint (Netgear AX1800 WiFi 6)
- `172.16.1.105` - tl-sg108e (TP-Link managed switch)
- `172.16.1.120` - pixel (Pixel phone, GrapheneOS)
- `172.16.1.220` - shield (Smart TV, Android)

### DMZ
- `10.0.1.120` - matrix (Matrix homeserver, Synapse)

### VPN Only
- `10.10.5.15 - pixel (via WireGuard)
- `10.10.5.115 - galaxy (Samsung, via WireGuard)
- `10.10.5.195 - joker-black (via WireGuard)

## Key Services by Device

| Device | Primary Services |
|--------|-----------------|
| wall-of-cards | pfSense, DHCP, firewall, WireGuard VPN |
| jack-of-diamonds | Pi-hole DNS, Nextcloud, Unifi Controller, Cloudflare tunnel |
| six-of-spades | n8n, Vaultwarden, Jellyfin, Homepage, Portainer |
| queen-of-diamonds | Plex, Sonarr, Radarr, Prowlarr, qBittorrent |
| king-of-diamonds | TrueNAS SCALE, NFS/SMB shares, ZFS pools |
| seven-of-spades | Immich photo server |
| three-of-diamonds | Ollama, OpenWebUI, AI/ML workloads |
| matrix | Matrix Synapse homeserver (DMZ isolated) |
| two-of-diamonds | Proxmox VE hypervisor |

## Common Questions & Where to Find Answers

**"What's running on device X?"** → [`devices/device_service_index.md`](../devices/device_service_index.md)

**"What's the network layout?"** → [`network/network_topology.md`](../network/network_topology.md)

**"What are the firewall rules?"** → [`network/firewall_rules.md`](../network/firewall_rules.md)

**"What's the IP/MAC of device X?"** → [`network/dhcp_configuration.md`](../network/dhcp_configuration.md)

**"What hardware does device X use?"** → [`hardware/device_hardware_index.md`](../hardware/device_hardware_index.md)

**"What's the device naming scheme?"** → [`hardware/naming_index.md`](../hardware/naming_index.md)

**"How do I navigate the vault?"** → [`README.md`](../README.md) (root)

**"What are the standards/conventions?"** → [`CLAUDE.md`](../CLAUDE.md) and [`documentation/homelab_guide.md`](homelab_guide.md)

**"What services map to what IPs?"** → [`network/service_matrix.md`](../network/service_matrix.md)

## Network Isolation Summary

**VPN (WireGuard)** → Full access to all networks

**LAN** → Full access to GUST and DMZ

**GUST** → Limited LAN access (DNS, NAS, AI server only), blocked from DMZ

**DMZ** → Strict isolation, DNS only + Matrix↔n8n bidirectional

See [firewall_rules.md](../network/firewall_rules.md) for complete rule details.

## File Locations for Key Topics

### Device Information
- [`devices/device_service_index.md`](../devices/device_service_index.md) - Master device table
- [`hardware/naming_index.md`](../hardware/naming_index.md) - Playing card naming
- [`hardware/device_hardware_index.md`](../hardware/device_hardware_index.md) - Hardware specs

### Network Configuration
- [`network/network_topology.md`](../network/network_topology.md) - Network layout
- [`network/dhcp_configuration.md`](../network/dhcp_configuration.md) - Static DHCP mappings
- [`network/firewall_rules.md`](../network/firewall_rules.md) - Firewall policies
- [`network/service_matrix.md`](../network/service_matrix.md) - Service-to-IP mapping
- [`network/dns_configuration.md`](../network/dns_configuration.md) - DNS records

## Critical Standards

**Device naming**: Playing card deck (diamonds, spades, hearts, clubs + jokers)
**File naming**: snake_case (e.g., `my_file_name.md`)
**Network segments**: 4 networks (LAN, GUST, DMZ, VPN)
**Primary DNS**: jack-of-diamonds (192.168.1.50) - Pi-hole
**Storage**: king-of-diamonds (192.168.1.62) - TrueNAS with 48TB ZFS
**Virtualization**: two-of-diamonds (192.168.1.2) - Proxmox VE 8.x

## Quick Stats

- **13 active devices** (excluding network switches/APs/modem)
- **4 network segments** with firewall isolation
- **3 Proxmox VMs** (king-of-diamonds, seven-of-spades, matrix)
- **3 Mini PCs** (jack-of-diamonds, six-of-spades, queen-of-diamonds)
- **3 Physical servers** (two-of-diamonds Proxmox, three-of-diamonds AI, ten-of-spades Gaming)
- **40+ containerized services** across infrastructure
- **48TB storage** across 4 ZFS pools
- **3 VPN peers** on WireGuard

## Related Files

- [Main README](../README.md) - Comprehensive vault overview
