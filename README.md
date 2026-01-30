# House of Cards 

I would like to take a moment and thank people that played very important role in my learning process (SEE Teachers Section). Their informative and engaging videos, articles and classes have simplified complex topics, making them accessible and enjoyable for me to grasp. Their dedication to sharing knowledge has had a major impact on my growth, and I am truly thankful for their valuable contributions. I encourage you to subscribe and contribute to their work. Best regards to all of you. 
## Home Lab

This repository contains the configuration files and documentation for additions and changes of my home lab previously partially documented in [House Of Cards](https://github.com/arm-ser/house-of-cards)

> **AI-Powered Documentation**: This homelab documentation is  maintained with assistance from [Claude Code](https://claude.com/claude-code).

> **Note**: All IP addresses, device names, and sensitive identifiers in this documentation have been randomized and anonymized for privacy and security purposes.

**Skills Demonstrated**:
- Network architecture & segmentation (4 isolated networks)
- Security hardening (firewall rules, DMZ isolation, VPN)
- Docker containerization & orchestration (40+ services)
- Multi-platform administration (Proxmox, TrueNAS, pfSense, Linux)

**Quick Stats**: 13 devices | 4 network segments | 40+ services | 48TB storage
→ See [documentation/quick_reference.md](documentation/quick_reference.md) for full inventory

---
## Network Architecture

The homelab utilizes **4 isolated network segments** managed by pfSense: **LAN** (192.168.1.0/24) for servers and infrastructure, **GUST** (172.16.1.0/24) for WiFi devices with limited access, **DMZ** (10.0.1.0/24) for strictly isolated public services, and **WireGuard VPN** (10.10.5.1/24) for secure remote access.

**Network Documentation**: [network_topology.md](network/network_topology.md) | [firewall_rules.md](network/firewall_rules.md) | [dhcp_configuration.md](network/dhcp_configuration.md)

```
                                   INTERNET
                                       |
                                       | WAN (10.0.1.89)
                                       |
                         ┌────────────────────────────┐
                         │   wall-of-cards (pfSense)  │
                         │    4x 2.5GbE Firewall      │
                         └────────────────────────────┘
                                       |
       ┌───────────────────────────────┼───────────────┬──────────────────┐
       │                       │                       │                  │
       │ LAN                   │ GUST                  │ DMZ              │ VPN
       │ 192.168.1.1           │ 172.16.1.1            │ 10.0.1.10        │ 10.10.5.1
       │ (igc2)                │ (igc1)                │ (igc3)           │ (tun_wg0)
       │                       │                       │                  │
┌──────┴─────┐           ┌─────┴─────┐          ┌──────┴──────┐     ┌─────┴───────┐
│   SWITCHES │           │  SWITCHES │          │   SWITCH    │     │  VPN PEERS  │
└──────┬─────┘           └─────┬─────┘          └──────┬──────┘     └─────┬───────┘
       │                       │                       │                  │
       │                       │                       │                  │
┌──────┴──────────┐     ┌──────┴──────────┐     ┌──────┴──────┐   ┌───────┴───────┐
│ LAN DEVICES     │     │ GUST DEVICES    │     │ DMZ DEVICES │   │ VPN CLIENTS   │
│ 192.168.1.0/24  │     │ 172.16.1.0/24   │     │ 10.0.1.0/24 │   │ 10.10.5.1/24  │
└─────────────────┘     └─────────────────┘     └─────────────┘   └───────────────┘

╔═════════════════════════════════════════════════════════════════════════════════╗
║                             LAN - 192.168.1.0/24                                ║
║                        Gateway: .1  |  DNS: .50 (Pi-hole)                       ║
╚═════════════════════════════════════════════════════════════════════════════════╝

┌─────────────────────────────────────────────────────────────────────────────────┐
│  .2   │ two-of-diamonds      │ Proxmox VE Host    │ i7-8700K, 32GB              │
│  .10  │ ten-of-spades        │ Gaming PC (LAN)    │ i7-9700K, RTX 5070, 32GB    │
│  .50  │ jack-of-diamonds     │ Primary Services   │ MeLE Mini PC (DNS)          │
│  .62  │ king-of-diamonds     │ TrueNAS Storage    │ Proxmox VM (48TB ZFS)       │
│  .100 │ six-of-spades        │ Multi-Purpose      │ CTONE Mini PC (n8n, etc)    │
│  .107 │ seven-of-spades      │ Photo Server       │ Proxmox VM (Immich)         │
│  .200 │ three-of-diamonds    │ AI/ML Server       │ i7-8700K, RTX 3090, 64GB    │
│  .250 │ queen-of-diamonds    │ Media Automation   │ GMKtec (Plex, *arr stack)   │
└─────────────────────────────────────────────────────────────────────────────────┘

╔═════════════════════════════════════════════════════════════════════════════════╗
║                            GUST - 172.16.1.0/24                                 ║
║                      Gateway: .1  |  DNS: 192.168.1.50                          ║
║                   Limited LAN Access (DNS, NAS, AI only)                        ║
╚═════════════════════════════════════════════════════════════════════════════════╝

┌─────────────────────────────────────────────────────────────────────────────────┐
│  .5   │ joker-black          │ Admin Workstation  │ Dell 16 Plus (Ubuntu/Win)   │
│  .10  │ ten-of-spades        │ Gaming PC (WiFi)   │ Dual-homed device           │
│  .15  │ guest-accesspoint    │ WiFi AP            │ Netgear AX1800 (WiFi 6)     │
│  .105 │ tl-sg108e            │ Managed Switch     │ TP-Link 8-port              │
│  .120 │ pixel                │ Mobile Device      │ Pixel (GrapheneOS)          │
│  .220 │ shield               │ Smart TV           │ Android TV                  │
└─────────────────────────────────────────────────────────────────────────────────┘

╔═════════════════════════════════════════════════════════════════════════════════╗
║                              DMZ - 10.0.1.0/24                                  ║
║                      Gateway: .10  |  DNS: 192.168.1.50                         ║
║                 Strict Isolation (DNS only + Matrix↔n8n)                        ║
╚═════════════════════════════════════════════════════════════════════════════════╝

┌─────────────────────────────────────────────────────────────────────────────────┐
│  .120 │ matrix               │ Matrix Homeserver  │ Proxmox VM (Synapse)        │
└─────────────────────────────────────────────────────────────────────────────────┘

╔═════════════════════════════════════════════════════════════════════════════════╗
║                       WireGuard VPN - 10.10.5.1/24                              ║
║                  Gateway: .1  |  Full Network Access                            ║
╚═════════════════════════════════════════════════════════════════════════════════╝

┌─────────────────────────────────────────────────────────────────────────────────┐
│  10.10.5.15  │ pixel (VPN)     │ Mobile Remote      │ Pixel (GrapheneOS)        │
│  10.10.5.115 │ galaxy (VPN)    │ Mobile Remote      │ Samsung Galaxy            │
│  10.10.5.195 │ joker-black (VPN)│ Admin Remote      │ Dell 16 Plus              │
└─────────────────────────────────────────────────────────────────────────────────┘

╔═════════════════════════════════════════════════════════════════════════════════╗
║                           NETWORK ISOLATION RULES                               ║
╠═════════════════════════════════════════════════════════════════════════════════╣
║  VPN  →  Full access to LAN, GUST, DMZ, Internet                                ║
║  LAN  →  Full access to GUST and DMZ                                            ║
║  GUST →  Limited LAN (DNS, NAS, AI), Blocked from DMZ                           ║
║  DMZ  →  DNS only + Matrix↔n8n bidirectional (strict isolation)                 ║
╚═════════════════════════════════════════════════════════════════════════════════╝
```

**Network Highlights**:
- **4 isolated network segments** with pfSense firewall routing
- **Dual-homed device**: ten-of-spades accessible on both LAN (.10) and GUST (.10)
- **Security layers**: DMZ strict isolation, GUST limited access, VPN full access
- **3 Proxmox VMs**: king-of-diamonds (TrueNAS), seven-of-spades (Immich), matrix (DMZ)

---

## Devices

**Hardware Specs**: [device_hardware_index.md](hardware/device_hardware_index.md) | **Service Mapping**: [device_service_index.md](devices/device_service_index.md)

| Device                                            | Role                 | Description                                         |
| ------------------------------------------------- | -------------------- | --------------------------------------------------- |
| [wall-of-cards](devices/wall_of_cards.md)         | Firewall/Router      | pfSense firewall managing 4 network segments        |
| [two-of-diamonds](devices/two_of_diamonds.md)     | Proxmox Host         | Virtualization host for TrueNAS, Immich, Matrix VMs |
| [jack-of-diamonds](devices/jack_of_diamonds.md)   | Primary DNS/Services | Pi-hole DNS, Vaultwarden, Uptime Kuma               |
| [king-of-diamonds](devices/king_of_diamonds.md)   | TrueNAS Storage      | 48TB ZFS storage server (Proxmox VM)                |
| [six-of-spades](devices/six_of_spades.md)         | Multi-Purpose Server | n8n automation, Portainer, backup services          |
| [seven-of-spades](devices/seven_of_spades.md)     | Photo Server         | Immich photo management (Proxmox VM)                |
| [three-of-diamonds](devices/three_of_diamonds.md) | AI/ML Server         | Ollama, Open WebUI, GPU workloads                   |
| [queen-of-diamonds](devices/queen_of_diamonds.md) | Media Automation     | Plex, Radarr, Sonarr, Prowlarr stack                |
| [matrix](devices/matrix.md)                       | Matrix Homeserver    | Synapse homeserver in isolated DMZ                  |
| [ten-of-spades](devices/ten_of_spades.md)         | Gaming/Workstation   | Dual-homed device (LAN + GUST)                      |
| [joker-black](devices/joker_black.md)             | Admin Workstation    | Primary admin laptop (Ubuntu/Windows dual-boot)     |

---

## Future Roadmap

> **Current State**: This homelab is in active development and represents a working production environment in its early maturity phase. While all core services are operational, significant security hardening and infrastructure improvements are planned.

### Phase 1: Security Foundation
1. **VPN Access Segmentation** - Implement granular firewall rules for WireGuard clients (currently unrestricted full access)
2. **Comprehensive Backup Strategy** - Deploy 3-2-1 backup rule with automated backups and offsite replication
3. **Authentication Hardening** - SSH key-only authentication, brute-force protection (fail2ban), and credential rotation
4. **Certificate Management** - Automate SSL/TLS certificates for internal services (Let's Encrypt)
### Phase 2: Monitoring & Threat Detection
5. **Intrusion Detection & Prevention** - Network-level threat detection and blocking (Snort/Suricata)
6. **Centralized Logging & Analysis** - Aggregate and analyze logs across infrastructure (Loki/OpenSearch)
7. **Metrics & Alerting** - System health monitoring with automated alerts (Prometheus + Grafana)
### Phase 3: Reliability & Automation
8. **High Availability** - Eliminate single points of failure with redundant critical services
9. **Infrastructure as Code** - Automate provisioning and configuration management (Ansible)
10. **Container Orchestration** - Enhanced service management and scaling capabilities (k3s/Kubernetes)


---

## Resources and Tools

| Tool<img width=182/>                                  | Description<img width=250/>           | Tutorials                                                                                                                                                                                                                  |
| ----------------------------------------------------- | ------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Tabby](https://github.com/Eugeny/tabby)              | Highly configurable terminal emulator | <img src="https://github.com/arm-ser/house-of-cards/blob/main/logos-screenshots/youtube.png?raw=true" width="15" /> [Tabby Tutorial](https://www.youtube.com/watch?v=-yfuYPowUDE)                                          |
| [Ventoy](https://www.ventoy.net/en/index.html)        | Easy Multiboot USB Creator            | <img src="https://github.com/arm-ser/house-of-cards/blob/main/logos-screenshots/youtube.png?raw=true" width="15" /> [Ventoy Tutorial](https://www.youtube.com/watch?v=-hs4mH7uBkk)<br>                                     |
| [Obsidian](https://obsidian.md/)                      | One of the best note-taking tools     | <img src="https://github.com/arm-ser/house-of-cards/blob/main/logos-screenshots/youtube.png?raw=true" width="15" /> [Obsidian Tutorial Playlist](https://www.youtube.com/playlist?list=PL5fd4SsfvECy0zzf8Cyo20ZoipEt6YeL3) |
| [Lightshot](https://app.prntscr.com/en/download.html) | Convenient Screenshot taking tool     |                                                                                                                                                                                                                            |

---

## Teachers 

<img src="https://github.com/arm-ser/house-of-cards/blob/main/logos-screenshots/youtube.png?raw=true" width="15" /> [Awesome Open Source](https://www.youtube.com/@AwesomeOpenSource/featured) <img src="https://github.com/arm-ser/house-of-cards/blob/main/logos-screenshots/youtube.png?raw=true" width="15" /> [Lawrence Systems](https://www.youtube.com/@LAWRENCESYSTEMS) <img src="https://github.com/arm-ser/house-of-cards/blob/main/logos-screenshots/youtube.png?raw=true" width="15" /> [DB Tech](https://www.youtube.com/@DBTechYT) <img src="https://github.com/arm-ser/house-of-cards/blob/main/logos-screenshots/youtube.png?raw=true" width="15" /> [Raid Owl](https://www.youtube.com/@RaidOwl) <img src="https://github.com/arm-ser/house-of-cards/blob/main/logos-screenshots/youtube.png?raw=true" width="15" /> [Ben Eater](https://www.youtube.com/@BenEater) <img src="https://github.com/arm-ser/house-of-cards/blob/main/logos-screenshots/youtube.png?raw=true" width="15" /> [IBRACORP](https://www.youtube.com/@IBRACORP) <img src="https://github.com/arm-ser/house-of-cards/blob/main/logos-screenshots/youtube.png?raw=true" width="15" /> [Techno Tim](https://www.youtube.com/@TechnoTim) <img src="https://github.com/arm-ser/house-of-cards/blob/main/logos-screenshots/youtube.png?raw=true" width="15" /> [Mark Furneaux](https://www.youtube.com/@TheUbuntuGuy) <img src="https://github.com/arm-ser/house-of-cards/blob/main/logos-screenshots/youtube.png?raw=true" width="15" /> [Christian Lempa](https://www.youtube.com/@christianlempa) <img src="https://github.com/arm-ser/house-of-cards/blob/main/logos-screenshots/youtube.png?raw=true" width="15" /> [John Hammond](https://www.youtube.com/@_JohnHammond)

<img src="https://github.com/arm-ser/house-of-cards/blob/main/logos-screenshots/youtube.png?raw=true" width="15" /> [Чёрный Треугольник](https://www.youtube.com/@Black_Triangle) <img src="https://github.com/arm-ser/house-of-cards/blob/main/logos-screenshots/youtube.png?raw=true" width="15" /> [DesignerMix](https://www.youtube.com/@DesignerMix) <img src="https://github.com/arm-ser/house-of-cards/blob/main/logos-screenshots/youtube.png?raw=true" width="15" /> [Нетипичный Безопасник](https://www.youtube.com/@MChannelone)  

<img src="https://github.com/arm-ser/house-of-cards/blob/main/logos-screenshots/linkedin.png?raw=true" width="15" /> [Sevada Isayan](https://www.linkedin.com/in/sevadaisayan/) <img src="https://github.com/arm-ser/house-of-cards/blob/main/logos-screenshots/linkedin.png?raw=true" width="15" /> [Gevorg Atoyan](https://www.linkedin.com/in/gevorgatoyan/)   
  
<img src="https://github.com/arm-ser/house-of-cards/blob/main/logos-screenshots/tumo.png?raw=true" width="15" /> [Tumo Center Of Creative Technologies](https://tumo.org/)

---

## Disclaimer
This home lab is intended strictly for educational and experimental purposes. Any use of information provided in this lab for illegal or unethical activities is strictly prohibited. The creator cannot be held responsible for any consequences resulting from misuse. By accessing and using information provided in this lab, you acknowledge and agree to comply with all applicable laws and ethical guidelines. Use this information responsibly and at your own risk.
