# Network Service Matrix

Complete service-to-IP mapping across all network segments.

## Overview

This matrix provides a quick reference for all services running in the homelab, organized by network segment. For detailed service information, see individual device pages linked below.

## Main LAN (192.168.1.0/24)

| IP Address    | Hostname   | Device                                   | Primary Services                                            | Key Dependencies              | Doc Status                      |
| ------------- | ---------- | ---------------------------------------- | ----------------------------------------------------------- | ----------------------------- | ------------------------------- |
| 192.168.1.1   | -          | VNOPN Firewall                           | Firewall/Router, DHCP, WireGuard VPN                        | -                             | [TBD](vnopn_firewall\.md)         |
| 192.168.1.2   | proxmox    | [Two of Diamonds](two_of_diamonds\.md)     | Proxmox VE hypervisor, 3 VMs                                | DNS, Gateway                  | [Complete](two_of_diamonds\.md)   |
| 192.168.1.50  | jack.dmnd  | [Jack of Diamonds](jack_of_diamonds\.md)   | Primary DNS (PiHole), NPM reverse proxy, 17 Docker services | Gateway                       | [Complete](jack_of_diamonds\.md)  |
| 192.168.1.51  | -          | Seven of Diamonds                        | TBD                                                         | TBD                           | Missing                         |
| 192.168.1.60  | six.dmnd   | [Six of Diamonds](six_of_diamonds\.md)     | Secondary DNS (PiHole)                                      | Primary DNS, Gateway          | [Complete](six_of_diamonds\.md)   |
| 192.168.1.62  | king.dmnd  | [King of Diamonds](king_of_diamonds\.md)   | TrueNAS storage (NFS/SMB)                                   | DNS, Gateway                  | [Partial](king_of_diamonds\.md)   |
| 192.168.1.100 | six.spds   | [Six of Spades](six_of_spades\.md)         | AI/LLM services, Jellyfin, 20 Docker services               | DNS, Gateway                  | [Complete](six_of_spades\.md)     |
| 192.168.1.150 | -          | Five of Diamonds                         | TBD                                                         | TBD                           | Missing                         |
| 192.168.1.200 | -          | [Three of Diamonds](three_of_diamonds\.md) | AI/ML workloads                                             | TBD                           | [TBD](three_of_diamonds\.md)      |
| 192.168.1.250 | queen.dmnd | [Queen of Diamonds](queen_of_diamonds\.md) | Media automation (Radarr, Sonarr, qBit, ProtonVPN)          | DNS, Gateway, TrueNAS storage | [Complete](queen_of_diamonds\.md) |

### Main LAN - Service Summary

**Infrastructure Services**:
- DNS: Primary (192.168.1.50), Secondary (192.168.1.60)
- Storage: TrueNAS (192.168.1.62)
- Virtualization: Proxmox (192.168.1.2)
- Reverse Proxy: NPM on Jack (192.168.1.50) and Six of Spades (192.168.1.100)

**Application Services**:
- Media Automation: Queen of Diamonds (192.168.1.250)
- Media Streaming: Jellyfin on Six of Spades (192.168.1.100)
- AI/LLM: Six of Spades (192.168.1.100), Three of Diamonds (192.168.1.200)
- Cloud Services: Nextcloud, Bitwarden, HedgeDoc, OnlyOffice on Jack (192.168.1.50)
- Monitoring: Uptime Kuma on Jack (192.168.1.50), Portainer agents on multiple hosts

## IoT/Guest Network (172.16.1.0/24)

| IP Address | Hostname | Device | Primary Services | Key Dependencies | Doc Status |
|------------|----------|--------|------------------|------------------|------------|
| 172.16.1.50 | - | [Ten of Spades](ten_of_spades\.md) | Home Assistant | Main LAN DNS (192.168.1.50) | [TBD](ten_of_spades\.md) |

### IoT/Guest - Service Summary

**Home Automation**:
- Home Assistant: Ten of Spades (172.16.1.50)

**Cross-Network Access**:
- DNS queries to Main LAN (192.168.1.50)
- TBD: Document firewall rules for Main LAN → IoT access

## DMZ Network (10.0.1.0/24)

| IP Address | Hostname | Device | Primary Services | Key Dependencies | Doc Status |
|------------|----------|--------|------------------|------------------|------------|
| 10.0.1.2 | proxmox | Two of Diamonds (bridge) | Network bridge for DMZ VMs | Gateway (10.0.1.1) | [Complete](two_of_diamonds\.md) |
| 10.0.1.120 | matrix | [Matrix Server](matrix\.md) | Matrix homeserver, Cloudflare tunnel | Main LAN DNS (192.168.1.50) | [Partial](matrix\.md) |

### DMZ - Service Summary

**Communication Services**:
- Matrix homeserver: Matrix (10.0.1.120)

**External Access**:
- Cloudflare tunnel on Matrix for internet access
- VPN access via WireGuard (10.10.5.1/24)

**Cross-Network Access**:
- DNS queries to Main LAN (192.168.1.50)
- TBD: Document firewall rules for Main LAN ↔ DMZ access

## WireGuard VPN (10.10.5.1/24)

| IP Range | Device Type | Primary Purpose | Access Permissions | Doc Status |
|----------|-------------|-----------------|-------------------|------------|
| 10.10.5.1 | VPN Gateway | PfSense WireGuard server | Full control | TBD |
| 10.10.5.1/24 | VPN Clients | Remote access for mobile/laptop devices | TBD | TBD |

### VPN - Service Summary

**Remote Access**:
- WireGuard server on PfSense (10.10.5.1)
- Client IP range: 10.10.5.1/24

**Active VPN Peers**:
- **pixel** (10.10.5.15) - Mobile device (Pixel Graphene OS)
- **galaxy** (10.10.5.115) - Mobile device (Samsung Galaxy)
- **joker-black** (10.10.5.195) - Admin Laptop

**Access Policies**:
- VPN clients have full access to LAN network (192.168.1.0/24)
- VPN clients can access DNS (PiHole: 192.168.1.50)
- Routing: All traffic through VPN when connected

## Service Dependencies

### Critical Infrastructure

**DNS Hierarchy**:
1. Primary DNS: Jack of Diamonds (192.168.1.50)
2. Secondary DNS: Six of Diamonds (192.168.1.60)
3. All devices point to 192.168.1.50 for DNS

**Storage**:
- King of Diamonds (192.168.1.62) provides NFS/SMB shares
- Used by: Queen of Diamonds (media storage), TBD (other consumers)

**Network Gateway**:
- PfSense (192.168.1.1) is gateway for all Main LAN devices
- Separate gateways for IoT (172.16.1.1) and DMZ (10.0.1.1)

### Cross-VLAN Communication

**Confirmed Cross-Network Dependencies**:
1. IoT → Main LAN: DNS queries (172.16.1.0/24 → 192.168.1.50)
2. DMZ → Main LAN: DNS queries (10.0.1.0/24 → 192.168.1.50)
3. TBD: Media automation → Storage (192.168.1.250 → 192.168.1.62)
4. TBD: Six of Spades Jellyfin → Storage
5. TBD: VPN clients → All networks?

**Actual Routing Policies** (from firewall audit 2025-11-25):
- [x] Main LAN → IoT/Guest: **Full access allowed**
- [x] Main LAN → DMZ: **Full access allowed**
- [x] IoT/Guest → Main LAN: **⚠️ Appears to allow all (logged)** - Rule 1711686678
- [x] IoT/Guest → DMZ: **Blocked**
- [x] DMZ → Main LAN: **DNS only** (192.168.1.50, 192.168.1.60 port 53)
- [x] DMZ → IoT/Guest: **Blocked**
- [x] VPN → All networks: **Full unrestricted access**

**Security Findings**:
- IoT/Guest has explicit allow rules for DNS, NAS, AI server, but also has "pass" rule for ALL Main LAN traffic (logged)
- Unknown device 10.0.1.220 in DMZ has Jellyfin access to 192.168.1.100
- WireGuard VPN clients have no restrictions (security concern)

### External Access

**Internet-Facing Services**:
- Cloudflare tunnel: Matrix server (10.0.1.120)
- WireGuard VPN: PfSense (termination point)
- All other services: Local-only

**VPN Access**:
- ProtonVPN outbound: Queen of Diamonds (192.168.1.250)
- ProtonVPN outbound: Six of Spades (192.168.1.100) via Gluetun

## Service Port Summary

### Core Network Services

| Service | Primary Host | Backup Host | Port(s) | Protocol |
|---------|-------------|-------------|---------|----------|
| DNS | 192.168.1.50 | 192.168.1.60 | 53 | TCP/UDP |
| DHCP | 192.168.1.1 | - | 67/68 | UDP |
| Gateway | 192.168.1.1 | - | - | - |
| NFS | 192.168.1.62 | - | 2049 | TCP |
| SMB | 192.168.1.62 | - | 445 | TCP |

### Application Services by Category

**Media Services**:
- Jellyfin (Six of Spades): 8096
- Radarr (Queen): 5200 (via ProtonVPN)
- Sonarr (Queen): 5300 (via ProtonVPN)
- qBittorrent (Queen): 5500 (via ProtonVPN)
- Navidrome (Six of Spades): 4533
- Audiobookshelf (Six of Spades): 8020

**AI/LLM Services**:
- LiteLLM Proxy (Six of Spades): 4000
- Open WebUI (Six of Spades): TBD
- Pipelines (Six of Spades): 9099

**Cloud/Productivity**:
- Nextcloud (Jack): via NPM
- Bitwarden (Jack): via NPM
- HedgeDoc (Jack): via NPM
- OnlyOffice (Jack): 8780
- Grist (Jack): 8484
- NocoDB (Jack): via NPM

**Automation**:
- n8n (Six of Spades): TBD
- Home Assistant (Ten of Spades): TBD

**Management**:
- Proxmox Web UI: 8006
- Portainer (Jack): 9443
- Portainer (Six of Spades): 9443
- Nginx Proxy Manager: 80, 81, 443 (multiple hosts)
- PiHole Admin: 5380

## Unknown Devices Found (Firewall Audit)

**From firewall rules audit (2025-11-25)**:

| IP Address | Network | Found In | Purpose | Action Required |
|------------|---------|----------|---------|-----------------|
| 10.0.1.220 | DMZ | Firewall rules | Jellyfin access to 192.168.1.100:80/443 | Document device, verify purpose |
| `<agent>` | Main LAN | Firewall alias | WOL and SSH restricted device | Identify IP, add to service matrix |
| `<shield>` | IoT/Guest | Firewall alias | Special WAN access (likely Home Assistant) | Verify if 172.16.1.50, document |

**Action**: Cross-reference these with DHCP leases and actual device inventory

## Documentation Status

### Complete ✓
- [Jack of Diamonds](../devices/jack_of_diamonds.md) - Full service list, ports, containers
- [Six of Spades](../devices/six_of_spades.md) - Full service list, ports, containers
- [Queen of Diamonds](../devices/queen_of_diamonds.md) - Full service list, VPN routing
- [Six of Diamonds](six_of_diamonds.md) - DNS configuration
- [Two of Diamonds](../devices/two_of_diamonds.md) - Proxmox host, VM details

### Partial ⚠
- [King of Diamonds](../devices/king_of_diamonds.md) - Missing: NFS/SMB share details, active services
- [Matrix](../devices/matrix.md) - Missing: Exact service configuration, backup strategy

### Missing/TBD ✗
- [Three of Diamonds](three_of_diamonds.md) - AI server not documented
- [Ten of Spades](../devices/ten_of_spades.md) - Home Assistant not documented
- Ten of Diamonds - Gaming PC not documented
- Seven of Diamonds (192.168.1.51) - Device not documented
- Five of Diamonds (192.168.1.150) - Device not documented
- [VNOPN Firewall](vnopn_firewall.md) - Needs detailed config, firewall rules
- WireGuard VPN - Client list and access policies missing

## Next Steps

### High Priority
1. **Document firewall rules** between network segments
2. **Fill King of Diamonds TBDs** - Active shares and services
3. **Document Three of Diamonds** - AI server services
4. **Map storage dependencies** - Which services use TrueNAS shares

### Medium Priority
5. **Document Home Assistant** setup (Ten of Spades)
6. **Create network flow diagram** showing cross-VLAN dependencies
7. **Verify DNS configurations** match documented setup
8. **Audit VPN access policies** and client configurations

### Low Priority
9. Document Ten of Diamonds (Gaming PC) if running services
10. Document Seven/Five of Diamonds when assigned
11. Create Obsidian canvas network topology diagram

## Related

- [Network Topology](network_topology.md) - Network segments and IP ranges
- [DNS Configuration](dns_configuration.md) - DNS records and PiHole setup
- [DHCP Configuration](dhcp_configuration.md) - DHCP server and static mappings
- [Firewall Rules](firewall_rules.md) - PfSense firewall policies
- [Device Service Index](device_service_index.md) - Device list with hardware details

---

[← Back to Network](_index.md)
