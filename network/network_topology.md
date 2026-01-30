# Network Topology

## Overview

The homelab network consists of 4 separate networks connected through a PfSense firewall/router:
- **Main LAN**: Primary network for servers and workstations
- **GUST**: Network for wireless devices and guest access
- **DMZ**: Demilitarized zone for isolated services
- **WireGuard VPN**: Remote access network for connecting devices when away from home

## Network Segments

### Main LAN (192.168.1.0/24)

**Subnet**: 192.168.1.0/24
**Gateway**: 192.168.1.1 (wall-of-cards PfSense Firewall)
**DNS**: 192.168.1.50 (Jack of Diamonds - PiHole)
**Purpose**: Primary network for homelab servers, management interfaces, and workstations

**Connected Devices**:
- 192.168.1.1 - [wall-of-cards](../devices/wall_of_cards.md) PfSense Firewall
- 192.168.1.10 - [Ten of Spades](../devices/ten_of_spades.md) (Gaming PC via Ethernet)
- 192.168.1.15 - lan-accesspoint (Netgear R7900P) [OFFLINE]
- 192.168.1.2 - [Two of Diamonds](../devices/two_of_diamonds.md) (Physical Server) - Proxmox VE Host
- 192.168.1.50 - [Jack of Diamonds](../devices/jack_of_diamonds.md) (MeLE Mini PC) - Primary Services Host / DNS
- 192.168.1.62 - [King of Diamonds](../devices/king_of_diamonds.md) (Proxmox VM) - TrueNAS Storage
- 192.168.1.100 - [Six of Spades](../devices/six_of_spades.md) (CTONE Mini PC) - Multi-Purpose Server
- 192.168.1.107 - [Seven of Spades](../devices/seven_of_spades.md) (Proxmox VM) - Immich Photo Server
- 192.168.1.200 - [Three of Diamonds](three_of_diamonds.md) (AI Server) - AI/ML Workloads
- 192.168.1.250 - [Queen of Diamonds](../devices/queen_of_diamonds.md) (GMKtec Mini PC) - Media Automation

### GUST Network (172.16.1.0/24)

**Subnet**: 172.16.1.0/24
**Gateway**: 172.16.1.1 (wall-of-cards PfSense Firewall)
**Interface**: igc1 (wall-of-cards)
**DNS**: 192.168.1.50 (Jack of Diamonds - PiHole)
**Purpose**: WiFi network for wireless devices, IoT, phones, laptops

**Connected Devices**:
- 172.16.1.5 - [Joker Black](../devices/joker_black.md) (Dell 16 Plus DB16250) - Admin Workstation (Dual Boot Ubuntu/Windows)
- 172.16.1.10 - [Ten of Spades](../devices/ten_of_spades.md) (Gaming PC via WiFi)
- 172.16.1.15 - guest-accesspoint (Netgear WiFi 6 AX1800)
- 172.16.1.100 - astx-laptop (ASTX Laptop) - Workstation
- 172.16.1.105 - tl-sg108e (TP-Link TL-SG108E) - Managed Switch
- 172.16.1.120 - Pixel (Phone)
- 172.16.1.220 - shield (Smart TV)

**Access Rules**:
- **Internet access**: Full outbound access allowed
- **LAN access**: Limited - DNS (192.168.1.50), NAS (192.168.1.62), AI Server (192.168.1.200)
- **Special access**: 172.16.1.5 and 172.16.1.10 have full access to six-spades (192.168.1.100)
- **DMZ access**: Blocked
- **WG820 access**: Blocked
- See [Firewall Rules](firewall_rules.md) for complete details

### DMZ Network (10.0.1.0/24)

**Subnet**: 10.0.1.0/24
**Gateway**: 10.0.1.10 (wall-of-cards PfSense Firewall)
**Interface**: igc3 (wall-of-cards)
**DNS**: 192.168.1.50 (Jack of Diamonds - PiHole)
**Purpose**: DMZ network for isolated services exposed via Cloudflare tunnel

**Connected Devices**:
- 10.0.1.15 - dmz-accesspoint [OFFLINE]
- 10.0.1.120 - [Matrix](../devices/matrix.md) (Proxmox VM) - Matrix Homeserver

**Access Rules**:
- **Internet access**: Full outbound access allowed
- **LAN access**: Strict - DNS only (192.168.1.50:53), Matrix ↔ n8n bidirectional (192.168.1.100:5678/8008)
- **GUST access**: Blocked
- **WG820 access**: Matrix API only (n8n via VPN can access 10.0.1.120:8008)
- **Isolation**: High - DMZ is well isolated from other networks
- See [Firewall Rules](firewall_rules.md) for complete details

### WireGuard VPN Network (10.10.5.1/24)

**Subnet**: 10.10.5.1/24
**Gateway**: 10.10.5.1 (wall-of-cards PfSense Firewall)
**Interface**: tun_wg0 (WireGuard tunnel)
**Tunnel Name**: Tunnel-820 / tun_wg0
**Listen Port**: 820
**DNS**: 192.168.1.50 (Jack of Diamonds - PiHole)
**Purpose**: Remote access VPN for connecting personal devices when away from home

**VPN Configuration**:
- **Protocol**: WireGuard
- **Server**: PfSense firewall (10.10.5.1)
- **Client IP Range**: 10.10.5.1/24
- **Active Peers**: 3

**Connected Peers**:
- 10.10.5.15/32 - pixel (Mobile - Pixel Graphene OS)
- 10.10.5.115/32 - galaxy (Mobile - Samsung Galaxy)
- 10.10.5.195/32 - [Joker Black](../devices/joker_black.md) (Dell 16 Plus - Admin Laptop)

**Access Rules**:
- **Full network access**: VPN clients can access ALL networks (LAN, GUST, DMZ, Internet)
- **Security concern**: No restrictions on VPN access (see [Firewall Rules](firewall_rules.md))

## Router/Firewall

**Device**: [wall-of-cards](../devices/wall_of_cards.md) PfSense Firewall
**Management IP**: 192.168.1.1 (LAN)
**WAN IP**: 10.0.1.89
**Interfaces**:
- **WAN** (igc0): 10.0.1.89 - Internet connection
- **LAN** (igc2): 192.168.1.1 - Main LAN (192.168.1.0/24)
- **GUST** (igc1): 172.16.1.1 - WiFi/Guest network (172.16.1.0/24)
- **DMZ** (igc3): 10.0.1.10 - DMZ network (10.0.1.0/24)
- **WG820** (tun_wg0): 10.10.5.1 - WireGuard VPN (10.10.5.1/24)

**Key Services**:
- DHCP Server (see [DHCP Configuration](dhcp_configuration.md))
- WireGuard VPN Server
- Firewall/NAT

## Network Switches

**Type**: Unmanaged switches
**Details**: TBD - Document switch models and port connections

## Inter-Network Routing

**Status**: Partial routing enabled
**Configuration**: TBD - Audit and document which networks can communicate

**Questions to Answer**:
- Can Main LAN access IoT/Guest network?
- Can Main LAN access DMZ?
- Can IoT/Guest access Main LAN?
- Can IoT/Guest access DMZ?
- Can DMZ access any other networks?
- Can WireGuard VPN access all networks or restricted subset?

## Internet Access

**External Services**:
- Cloudflare Tunnels for secure external access (see [Cloudflare Tunnels](cloudflare_tunnels.md))
- No direct port forwarding
- VPN access via WireGuard

**Outbound Internet**:
- All networks have outbound internet access (verify)
- DNS filtering via PiHole on Main LAN

## Network Diagram

```
Internet
   |
   | (WAN)
   |
[wall-of-cards PfSense Firewall]
   |
   +---- LAN: 192.168.1.1 -------[Unmanaged Switch]---- Main LAN
   |                                                      (192.168.1.0/24)
   |                                                      - Servers
   |                                                      - Workstations
   |                                                      - PiHole DNS
   |
   +---- GUST: 172.16.1.1 ----[Unmanaged Switch]---- GUST Network
   |                                                      (172.16.1.0/24)
   |                                                      - Wireless Devices
   |                                                      - Workstations
   |                                                      - Guest WiFi
   |
   +---- DMZ: 10.0.1.10 ------[Unmanaged Switch]---- DMZ Network
   |                                                      (10.0.1.0/24)
   |                                                      - Isolated Services
   |
   +---- WireGuard: 10.10.5.1 -------------------------- Remote Clients
                                                          (10.10.5.1/24)
                                                          - Mobile devices
                                                          - Laptops
```

## Planned Network Changes

- [x] Document exact inter-network routing rules (completed 2026-01-28)
- [x] Audit and document firewall rules between networks (completed 2026-01-28)
- [x] Document WireGuard client access policies (completed 2026-01-28)
- [x] Create detailed network topology diagram (Obsidian canvas) (completed 2026-01-29)
- [ ] Document switch models and port assignments
- [ ] Configure DNS for GUST network
- [ ] Configure DNS for DMZ network
- [ ] Document WiFi access points and SSIDs
- [ ] Test and verify network isolation between segments

## Related

- [DNS Configuration](dns_configuration.md)
- [DHCP Configuration](dhcp_configuration.md)
- [Firewall Rules](firewall_rules.md)
- [Device Service Index](device_service_index.md)

---

[← Back to Network](_index.md)
