# Wall of Cards (pfSense Firewall)

## Notes

- Primary network firewall and router for entire homelab
- Manages 4 networks: WAN, LAN (192.168.1.0/24), GUST (172.16.1.0/24), DMZ (10.0.1.0/24)
- Runs DHCP server for all internal networks
- Handles inter-network routing and firewall policies
- WireGuard VPN server for remote access
- Power consumption: 15W (very efficient)
- Internet-exposed on WAN interface, management on LAN only
- Uptime critical - all network traffic depends on this device

## Device Information

**Card Name**: Wall of Cards
**Hardware**: Intel N3700 Firewall Appliance
**CPU**: Intel N3700 Quad Core (Up to 1.6GHz)
**RAM**: 8GB DDR3
**Storage**: 128GB mSATA SSD
**Power Draw**: 15W
**Network**: 4x Intel 2.5GbE i225 LAN Ports

**Management Interface**: Web UI (HTTPS port 443), SSH (port 22)

## Network Interfaces

### WAN 
- Connected to ISP modem
- Public IP (dynamic)
- Gateway to internet

### LAN 
- IP: 192.168.1.1/24
- Primary network for servers and infrastructure
- DHCP enabled

### GUST 
- IP: 172.16.1.1/24
- Guest/wireless network
- DHCP enabled

### DMZ
- IP: 10.0.1.1/24
- Isolated network for IoT and semi-trusted services
- DHCP enabled

## Operating System

**OS**: pfSense (FreeBSD-based)

## Running Services

### Core Services

| Service        | Purpose               | Port        |
| -------------- | --------------------- | ----------- |
| pfSense Web UI | Firewall management   | 443         |
| DHCP Server    | IP address assignment | 67 (UDP)    |
| WireGuard VPN  | Remote VPN access     | 55555 (UDP) |

### Firewall Functions

- Inter-network routing and isolation
- NAT for all internal networks
- Port forwarding rules
- VPN gateway for WireGuard peers

## VPN Configuration

**WireGuard Server**: wg0
- VPN network for remote access
- Provides access to all internal networks
- Multiple active peers (laptops, mobile devices)

## Related

- [Device Service Index](device_service_index.md)
- [Hardware Specs](../hardware/device_hardware_index.md)
- [Network Topology](../network/network_topology.md)
- [Firewall Rules](../network/firewall_rules.md)

---

[← Back to Device Index](device_service_index.md)
