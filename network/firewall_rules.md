# Firewall Rules

## Summary

**Security Posture**: Moderate isolation with some security concerns
**Last Audited**: 2026-01-28
**Rule Count**: ~30+ active rules across 5 interfaces

### Key Findings

**✓ Good Security**:
- WAN properly secured (bogon blocking, RFC1918 blocking, default deny)
- IOT (DMZ) well isolated with strict access controls
- NetBird Gateway container properly restricted (internet-only + DNS)
- Matrix homeserver has bidirectional communication with n8n

**⚠️ Security Concerns**:
1. **WireGuard Full Access**: WG820 has unrestricted access to ALL networks
2. **GUST Network Access**: Two devices (172.16.1.5, 172.16.1.10) have broad LAN access
3. **Shield Alias**: Unrestricted WAN access for unknown device

**🔍 Items to Investigate**:
- `agent` alias on Main LAN - WOL and SSH access only
- `shield` alias on GUST - full WAN access allowed
- NetBird Gateway (192.168.1.80) - LXC container with internet-only access

## Firewall Platform

**Device**: VNOPN PfSense Firewall
**WAN IP**: 10.0.1.89/20 (gateway: 47.28.112.1)
**Management IP**: 192.168.1.1 (Main LAN)
**Management Interface**: Web GUI (HTTPS port 10443, HTTP port 80)
**Interfaces**:
- `igc0` - WAN (Internet)
- `igc2` - LAN (192.168.1.0/24) - Main homelab network
- `igc1` - GUST (172.16.1.0/24) - Guest network
- `igc3` - IOT (10.0.1.0/24) - DMZ network
- `tun_wg0` - WG820 (10.10.5.1/24) - WireGuard VPN
- `tun_wg1` - WGOUT (unused)

## Rule Philosophy

**Default Policy**: Default deny all IPv4/IPv6 (block drop in/out log)
**Logging**: Enabled on block rules and critical allow rules
**Security Features**:
- Bogon network blocking on WAN
- Private network blocking on WAN (RFC1918)
- Port 0 blocking
- Link-local blocking (169.254.0.0/16)

## Interface Rules

### WAN Interface (igc0)

**Description**: Internet-facing interface (10.0.1.89/20)
**Default Policy**: Block all inbound, allow outbound from firewall itself
**Security**: Bogon blocking, RFC1918 blocking, anti-spoofing

#### Inbound Rules

| #   | Action | Protocol | Source         | Destination  | Port | Description               | Rule ID    |
| --- | ------ | -------- | -------------- | ------------ | ---- | ------------------------- | ---------- |
| 1   | Block  | All      | Bogons         | Any          | -    | Block bogon IPv4 networks | 11001      |
| 2   | Block  | All      | Bogons IPv6    | Any          | -    | Block bogon IPv6 networks | 11002      |
| 3   | Block  | All      | 10.0.0.0/8     | Any          | -    | Block private 10/8        | 12001      |
| 4   | Block  | All      | 127.0.0.0/8    | Any          | -    | Block loopback 127/8      | 12002      |
| 5   | Block  | All      | 172.16.0.0/12  | Any          | -    | Block private 172.16/12   | 12003      |
| 6   | Block  | All      | 192.168.0.0/16 | Any          | -    | Block private 192.168/16  | 12004      |
| 7   | Block  | All      | fc00::/7       | Any          | -    | Block ULA IPv6            | 12005      |
| 8   | Allow  | UDP      | Any            | 10.0.1.89 | 820  | WireGuard VPN inbound     | 1706627089 |
| 9   | Allow  | UDP      | Any            | 10.0.1.89 | 1194 | OpenVPN inbound           | 1749059232 |

#### Outbound Rules

**Policy**: Allow all outbound from firewall itself
**NAT**: Automatic outbound NAT enabled

### LAN Interface (igc2 - Main LAN 192.168.1.0/24)

**Description**: Primary homelab network
**Default Policy**: Allow LAN to any (including internet and other VLANs)
**Anti-Lockout**: Enabled for web GUI on ports 10443 and 80

#### Rules

| # | Action | Protocol | Source | Destination | Port | Description |
|---|--------|----------|--------|-------------|------|-------------|
| 1 | Allow | All | Any | LAN Address | 10443, 80 | Anti-Lockout Rule |
| 2 | Allow | TCP/UDP | 192.168.1.80 | 192.168.1.50 | 53 (DNS) | NetBird Gateway to DNS |
| 3 | Allow | ICMP | 192.168.1.80 | Any | - | NetBird Gateway Allow ICMP |
| 4 | Allow | TCP | 192.168.1.80 | 192.168.1.107 | 2283 | NetBird Gateway to Immich |
| 5 | Allow | All | 192.168.1.80 | ! Internal_Networks | - | Allow NetBird to internet only |
| 6 | Block | All | 192.168.1.80 | Any | - | Block NetBird Gateway to LAN services |
| 7 | Allow | TCP | 192.168.1.100 | 10.0.1.120 | 8008 | n8n to Matrix API access |
| 8 | Allow | TCP | `agent` | LAN subnets | 9 | Allow WOL packets from Agent |
| 9 | Allow | TCP | `agent` | LAN subnets | 22 (SSH) | Allow SSH To Agent |
| 10 | Block | TCP | `agent` | Any | - | Block ALL traffic from Agent |
| 11 | Allow | All | 192.168.1.0/24 | 172.16.1.0/24 | - | Allow All Traffic from LAN to GUST |
| 12 | **Allow** | **All** | **LAN subnets** | **Any** | **Any** | **Default allow LAN to any rule** |

**Notes**:
- NetBird Gateway (192.168.1.80) is restricted: DNS + Immich + Internet only
- `agent` alias has limited access: WOL (port 9), SSH (port 22), everything else blocked
- n8n server (192.168.1.100) can communicate with Matrix (10.0.1.120:8008)
- Rule #12 is the default rule allowing full LAN access to internet and all other networks

### IOT Interface (igc3 - DMZ 10.0.1.0/24)

**Description**: Isolated DMZ network for Matrix homeserver
**Default Policy**: Strict isolation with bidirectional Matrix ↔ n8n communication
**Isolation**: High - only Matrix API and DNS allowed

#### Rules

| # | Action | Protocol | Source | Destination | Port | Description |
|---|--------|----------|--------|-------------|------|-------------|
| 1 | Allow | TCP | 10.0.1.120 | 192.168.1.100 | 5678 | matrix to n8n |
| 2 | Allow | TCP | 10.0.1.120 | 192.168.1.100 | 8008 | Matrix responses to n8n |
| 3 | Allow | TCP | 10.0.1.120 | WG820 subnets | 8008 | Matrix responses to n8n |
| 4 | Allow | TCP/UDP | IOT subnets | 192.168.1.50 | 53 (DNS) | Allow DNS Traffic to server on LAN |
| 5 | **Block** | **All** | **Any** | **192.168.1.0/24** | **-** | **Block All Traffic to LAN** |
| 6 | **Block** | **All** | **Any** | **172.16.1.0/24** | **-** | **Block All Traffic to GUST** |
| 7 | **Block** | **All** | **Any** **10.10.5.1/24** | **-** | **Block All Traffic to WG820** |
| 8 | **Allow** | **All** | **Any** | **Any** | **-** | **Guest Network Allow Traffic Rule** |

**Notes**:
- Matrix homeserver (10.0.1.120) has bidirectional communication with n8n (192.168.1.100)
- Matrix can communicate on port 8008 (API) and 5678 (n8n webhook)
- DNS access to 192.168.1.50 allowed
- All other LAN, GUST, and WG820 traffic blocked
- Default rule allows internet access

### GUST Interface (igc1 - Guest Network 172.16.1.0/24)

**Description**: Guest network and secondary devices
**Default Policy**: Limited LAN access, allow internet
**Isolation**: Moderate - specific devices have LAN access

#### Rules

| # | Action | Protocol | Source | Destination | Port | Description |
|---|--------|----------|--------|-------------|------|-------------|
| 1 | Allow | TCP | 172.16.1.5 | 192.168.1.100 | - | joker-black to six-spades |
| 2 | Allow | TCP | 172.16.1.10 | 192.168.1.100 | - | ten-spades to six-spades |
| 3 | Allow | TCP/UDP | GUST subnets | 192.168.1.50 | 53 (DNS) | Allow DNS Traffic to server on LAN |
| 4 | Allow | TCP/UDP | GUST subnets | 192.168.1.62 | - | Allow Traffic to NAS on LAN |
| 5 | Allow | TCP/UDP | GUST subnets | 192.168.1.200 | - | Allow Traffic to AI Server on LAN |
| 6 | Allow | TCP | `shield` | WAN subnets | - | Allow Shield Traffic to WAN |
| 7 | **Block** | **All** | **Any** | **192.168.1.0/24** | **-** | **Block All Traffic to LAN** |
| 8 | **Block** | **All** | **Any** **10.10.5.1/24** | **-** | **Block All Traffic to WG820** |
| 9 | **Allow** | **All** | **Any** | **Any** | **-** | **Guest Network Allow Traffic Rule** |

**Notes**:
- Two specific devices (172.16.1.5, 172.16.1.10) have full access to six-spades (192.168.1.100)
- GUST devices can access: DNS (192.168.1.50), NAS (192.168.1.62), AI Server (192.168.1.200)
- `shield` alias has unrestricted WAN access
- GUST is blocked from: Main LAN (except specific rules), WG820 VPN
- GUST can reach internet (rule #9)

### WG820 Interface (WireGuard VPN - 10.10.5.1/24)

**Description**: Remote access VPN network (Port 820 on WAN)
**Default Policy**: Allow ALL traffic from VPN clients
**Isolation**: None - full network access

#### Rules

| # | Action | Protocol | Source | Destination | Port | Description |
|---|--------|----------|--------|-------------|------|-------------|
| 1 | **Allow** | **All** | **Any** | **Any** | **All** | **WG820 Allow Traffic** |
| 2 | Allow | TCP | WG820 subnets | 10.0.1.120 | 8008 | n8n to Matrix API access |

**Notes**:
- **Security Concern**: VPN clients have unrestricted access to ALL networks
- VPN can reach: Main LAN, GUST, IOT/DMZ, Internet
- n8n (via VPN) can communicate with Matrix homeserver (10.0.1.120:8008)
- **Recommendation**: Consider restricting VPN access to specific services/IPs

## NAT Rules

### Port Forwarding

**Note**: No direct port forwarding - using Cloudflare Tunnels for external access

| # | Interface | Protocol | External Port | Internal IP | Internal Port | Description | Status |
|---|-----------|----------|---------------|-------------|---------------|-------------|--------|
| N/A | N/A | N/A | N/A | N/A | N/A | No port forwarding configured | Active |

### Outbound NAT

**Mode**: Automatic (default pfSense configuration)
**Status**: Not customized - using pfSense defaults

## Floating Rules

**Status**: None configured - using interface-specific rules only

## Special Rules

### Anti-Lockout Rule

**Status**: TBD
**Description**: Prevents lockout from management interface
**Interface**: LAN
**Action**: Allow access to PfSense web interface from LAN

### Cloudflare Tunnel Rules

**Description**: Rules allowing Cloudflare Tunnel connectivity
**Status**: TBD - Document specific rules for Cloudflare Tunnel

See [Cloudflare Tunnels](cloudflare_tunnels.md) for detailed tunnel configuration.

## Network Isolation Matrix

| Source Network | → LAN | → GUST | → IOT/DMZ | → WG820 VPN | → Internet |
|----------------|-------|--------|-----------|-------------|------------|
| **LAN** | ✓ | ✓ | ✓ | ✓ | ✓ |
| **GUST** | ⚠️ Limited* | ✓ | ✗ | ✗ | ✓ |
| **IOT/DMZ** | ⚠️ Strict** | ✗ | ✓ | ⚠️ Matrix | ✓ |
| **WG820 VPN** | ✓ | ✓ | ✓ | ✓ | ✓ |

**Legend**:
- ✓ = Full access allowed
- ✗ = Blocked
- ⚠️ = Restricted/limited access (see notes)

**Isolation Details**:

\* **GUST → LAN (Limited)**:
  - Allowed: DNS (192.168.1.50), NAS (192.168.1.62), AI Server (192.168.1.200)
  - Special: 172.16.1.5 and 172.16.1.10 have full access to 192.168.1.100
  - Blocked: All other LAN access

\*\* **IOT/DMZ → LAN (Strict)**:
  - Allowed: DNS to 192.168.1.50 on port 53
  - Special: Matrix (10.0.1.120) can communicate with n8n (192.168.1.100) on ports 5678/8008
  - Blocked: All other LAN access

## Security Policies

### Blocked Traffic

**Bogon Networks**: Enabled on WAN interface (IPv4 and IPv6)
**Private Networks on WAN**: Blocked (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16, 127.0.0.0/8, fc00::/7)
**Geo-blocking**: Not configured

### IDS/IPS

**Status**: Not configured
**Snort/Suricata**: Not installed

## Monitoring and Alerts

**Log Location**: PfSense > Status > System Logs > Firewall
**Alert Rules**: Not configured
**Log Retention**: Default pfSense settings

## Related

- [Network Topology](network_topology.md)
- [VNOPN Firewall](vnopn_firewall.md)
- [DNS Configuration](dns_configuration.md)

---

[← Back to Network](_index.md)
