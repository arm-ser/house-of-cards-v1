# DHCP Configuration

## DHCP Server

**Primary DHCP**: VNOPN PfSense Firewall
**Service**: PfSense DHCP Server
**Management**: PfSense Web Interface (https://192.168.1.1)

## DHCP Scopes by Network

### Main LAN (192.168.1.0/24)

**DHCP Server IP**: 192.168.1.1
**Subnet**: 192.168.1.0/24 (192.168.1.1 - 192.168.1.254)
**Netmask**: 255.255.255.0
**Gateway**: 192.168.1.1
**DNS Servers**: 192.168.1.50 (PiHole on Jack of Diamonds)

**DHCP Pool Range**: 192.168.1.205 - 192.168.1.249 (45 IPs)
**Static Assignments**: 192.168.1.1 - 192.168.1.204 (reserved for infrastructure)
**Lease Time**: TBD

#### Static DHCP Mappings (Main LAN)

| Hostname          | IP Address    | Device            | Notes                          |
| ----------------- | ------------- | ----------------- | ------------------------------ |
| proxmox           | 192.168.1.2   | Two of Diamonds   | Proxmox VE Host                |
| ten-of-spades     | 192.168.1.10  | Ten of Spades     | Gaming PC                      |
| lan-accesspoint   | 192.168.1.15  | Netgear R7900P    | Main LAN WiFi AP               |
| jack-of-diamonds  | 192.168.1.50  | Jack of Diamonds  | Primary Services and DNS Host  |
| king-of-diamonds  | 192.168.1.62  | King of Diamonds  | TrueNAS Storage VM             |
| six-of-spades     | 192.168.1.100 | Six of Spades     | Multi-Purpose Container Server |
| seven-of-spades   | 192.168.1.107 | Seven of Spades   | Immich Photo Server VM         |
| three-of-diamonds | 192.168.1.200 | Three of Diamonds | AI ML Server                   |
| queen-of-diamonds | 192.168.1.250 | Queen of Diamonds | Media Automation Server        |

### GUST Network (172.16.1.0/24)

**DHCP Server IP**: 172.16.1.1
**Subnet**: 172.16.1.0/24 (172.16.1.1 - 172.16.1.254)
**Netmask**: 255.255.255.0
**Gateway**: 172.16.1.1
**DNS Servers**: 192.168.1.50 (Jack of Diamonds - PiHole)

**DHCP Pool Range**: 172.16.1.18 - 172.16.1.98 (81 IPs)
**Static Assignments**: Reserved IPs outside pool for infrastructure
**Lease Time**: TBD

#### Static DHCP Mappings (GUST)

| Hostname          | IP Address    | Device             | Notes                       |
| ----------------- | ------------- | ------------------ | --------------------------- |
| joker-black       | 172.16.1.5   | Joker Black        | Admin Laptop Ubuntu Windows |
| ten-of-spades     | 172.16.1.10  | Ten of Spades      | Gaming PC WiFi              |
| guest-accesspoint | 172.16.1.15  | Guest Access Point | Guest WiFi Access Point     |
| astx-laptop       | 172.16.1.100 | ASTX Laptop        | Astx's Laptop               |
| tl-sg108e         | 172.16.1.105 | TP-Link TL-SG108E  | GUST Network 8 Port Switch  |
| Pixel             | 172.16.1.120 | Pixel Phone        | Pixel Phone                 |
| shield            | 172.16.1.220 | Shield             | Nvidia Smart TV Box         |

### DMZ Network (10.0.1.0/24)

**DHCP Server IP**: 10.0.1.1
**Subnet**: 10.0.1.0/24 (10.0.1.1 - 10.0.1.254)
**Netmask**: 255.255.255.0
**Gateway**: 10.0.1.1
**DNS Servers**: 192.168.1.50 (Jack of Diamonds - PiHole)

**DHCP Pool Range**: 10.0.1.150 - 10.0.1.160 (11 IPs)
**Static Assignments**: 10.0.1.1 - 10.0.1.149 (reserved for infrastructure)
**Lease Time**: TBD

#### Static DHCP Mappings (DMZ)

| Hostname        | IP Address    | Device               | Notes                    |
| --------------- | ------------- | -------------------- | ------------------------ |
| dmz-accesspoint | 10.0.1.15  | TP-Link Access Point | DMZ WiFi Access Point    |
| matrix          | 10.0.1.120 | Matrix               | Matrix Server in Proxmox |

### WireGuard VPN Network (10.10.5.1/24)

**Note**: WireGuard uses static peer configuration (not DHCP)

**Subnet**: 10.10.5.1/24
**VPN Server**: 10.10.5.1
**Tunnel Name**: Tunnel-820 (tun_wg0)
**Listen Port**: 820
**Client IP Assignment**: Static configuration in WireGuard peer configs

#### VPN Peer Assignments

| Device Name | VPN IP Address | Allowed IPs   | Endpoint | Notes                                   |
| ----------- | -------------- | ------------- | -------- | --------------------------------------- |
| pixel       | 10.10.5.15/32   | 10.10.5.15/32  | Dynamic  | Mobile device (Pixel Graphene OS)       |
| galaxy      | 10.10.5.115/32  | 10.10.5.115/32 | Dynamic  | Mobile device (Samsung Galaxy)          |
| joker-black | 10.10.5.195/32  | 10.10.5.195/32 | Dynamic  | Admin Laptop (Dual Boot Ubuntu/Windows) |

## Related

- [Network Topology](network_topology.md)
- [DNS Configuration](dns_configuration.md)
- [Device Service Index](device_service_index.md)
- [VNOPN Firewall](vnopn_firewall.md)

---

[← Back to Network](_index.md)
