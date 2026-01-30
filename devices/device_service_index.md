# Device Service Index

Quick reference for all devices running services in the homelab.

## Network Infrastructure

| Device Name                                        | Hardware                | OS      | Primary Role         | IP Address      |
| -------------------------------------------------- | ----------------------- | ------- | -------------------- | --------------: |
| [Wall of Cards](wall_of_cards\.md)                   | Intel N3700 Firewall    | pfSense | Firewall/Router      |    192.168.1.1  |
| [Network Infrastructure](network_infrastructure\.md) | Switches, APs, Modem    | N/A     | Network Hardware     | Various         |

## Physical Servers

| Device Name                              | Hardware        | OS           | Primary Role          | IP Address      |
| ---------------------------------------- | --------------- | ------------ | --------------------- | --------------: |
| [Two of Diamonds](two_of_diamonds\.md)     | Custom NAS Build| Proxmox VE   | Virtualization Host   |    192.168.1.2  |
| [Three of Diamonds](three_of_diamonds\.md) | AI Server       | Ubuntu 24.04 | AI/ML Workloads       |  192.168.1.200  |

## Mini PCs

| Device Name                                | Hardware       | OS           | Primary Role         |    IP Address |
| ------------------------------------------ | -------------- | ------------ | -------------------- | ------------: |
| [Jack of Diamonds](jack_of_diamonds\.md)   | MeLE Mini PC   | Ubuntu 24.04 | Primary Services/DNS |  192.168.1.50 |
| [Six of Spades](six_of_spades\.md)         | CTONE Mini PC  | Ubuntu 24.04 | Multi-Purpose Server | 192.168.1.100 |
| [Queen of Diamonds](queen_of_diamonds\.md) | GMKtec Mini PC | Ubuntu 22.04 | Media Automation     | 192.168.1.250 |

## Virtual Machines

| Device Name                              | Hardware        | OS           | Primary Role          | IP Address      |
| ---------------------------------------- | --------------- | ------------ | --------------------- | --------------: |
| [King of Diamonds](king_of_diamonds\.md)   | Proxmox VM      | TrueNAS SCALE| Storage/NAS           |   192.168.1.62  |
| [Seven of Spades](seven_of_spades\.md)     | Proxmox VM      | Ubuntu 24.04 | Immich Photo Server   |  192.168.1.107  |
| [Matrix Server](matrix\.md)                | Proxmox VM      | Ubuntu 22.04 | Matrix Homeserver     | 10.0.1.120   |

## Workstations

| Device Name                              | Hardware        | OS                  | Primary Role          | IP Address      |
| ---------------------------------------- | --------------- | ------------------- | --------------------- | --------------: |
| [Ten of Spades](ten_of_spades\.md)         | Gaming PC       | Windows 11          | Gaming/Workstation    | 192.168.1.10 / 172.16.1.10 |
| [Joker Black](joker_black\.md)             | Dell 16 Plus    | Ubuntu 24.04/Win 11 | Admin Workstation     | 172.16.1.5   |

## Mobile & Other Devices

| Device Name                | Hardware           | OS          | Primary Role        | IP Address      |
| -------------------------- | ------------------ | ----------- | ------------------- | --------------: |
| [Pixel](pixel\.md)           | Pixel Phone        | Graphene OS | Mobile              | 172.16.1.120 |
| [Galaxy](galaxy\.md)         | Samsung Galaxy     | Android     | Mobile              | VPN only        |
| [Shield](shield\.md)         | Smart TV           | Android TV  | Entertainment       | 172.16.1.220 |

## Service Distribution Summary

**Network Services**: wall-of-cards (DHCP, DNS, VPN, Firewall)
**Primary DNS**: jack-of-diamonds (Pi-hole)
**Storage**: king-of-diamonds (TrueNAS with ZFS pools)
**Media**: queen-of-diamonds (Plex, *arr stack)
**Photos**: seven-of-spades (Immich)
**Communication**: matrix (Matrix/Synapse)
**AI/ML**: three-of-diamonds (Ollama, OpenWebUI, N8N)
**Multi-purpose**: six-of-spades (Various services)

## Related

- [Hardware Inventory](../hardware/device_hardware_index.md)
- [Device Naming Index](../hardware/naming_index.md)
- [Network Topology](../network/network_topology.md)
- [DHCP Configuration](../network/dhcp_configuration.md)
- [Services Index](_index.md)

---

[← Back to Services](_index.md)
