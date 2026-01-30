# House of Cards 

Yet again, I would like to take a moment and thank people that played very important role in my learning process (SEE Teachers Section). Their informative and engaging videos, articles and classes have simplified complex topics, making them accessible and enjoyable for me to grasp. Their dedication to sharing knowledge has had a major impact on my growth, and I am truly thankful for their valuable contributions. I encourage you to subscribe and contribute to their work. Best regards to all of you. 
## Home Lab

This repository contains the configuration files and documentation for additions and changes of my home lab previously partially documented in [House Of Cards](https://github.com/arm-ser/house-of-cards)

**Skills Demonstrated**:
- Network architecture & segmentation (4 isolated networks with security policies)
- Security hardening (firewall rules, DMZ isolation, VPN)
- Docker containerization & orchestration (40+ services)
- Infrastructure as Code practices
- Multi-platform system administration (Proxmox, TrueNAS, pfSense, Linux)
- Comprehensive technical documentation

**Evolution from Version 0**:
- Version 0: Hardware-focused documentation (what's running where)
- Version 1: Architecture-focused documentation (how it all works together)

---
## Quick Stats

- **Devices**: 13 active devices (3 physical servers, 3 mini PCs, 3 VMs, 2 workstations, 2 mobile)
- **Network Segments**: 4 networks (LAN, GUST, DMZ, WireGuard VPN)
- **Services**: 40+ containerized services across infrastructure
- **Storage**: 48TB across 4 ZFS pools
## Network Architecture

### LAN (192.168.1.0/24)
Primary network for servers and infrastructure. Gateway: 192.168.1.1 | DNS: 192.168.1.50

**Key Devices**: Proxmox host (.2), Primary DNS (.50), TrueNAS (.62), Multi-purpose server (.100), Photo server (.107), AI server (.200), Media automation (.250)

### GUST (172.16.1.0/24)
WiFi network for wireless devices and workstations. Gateway: 192.168.1.1 | DNS: 192.168.1.50

**Key Devices**: Admin laptop (.5), Gaming PC (.10), Guest AP (.15), Mobile devices

### DMZ (10.0.1.0/24)
Isolated network for services exposed via Cloudflare tunnel. Gateway: 192.168.1.1 | DNS: 192.168.1.50

**Key Devices**: Matrix homeserver (.120)
### WireGuard VPN (10.10.5.1/24)
Remote access VPN with full network access. Gateway: 192.168.1.1 | DNS: 192.168.1.50

**Connected Peers**: 3 devices (pixel, galaxy, joker-black)

## Network Topology Diagram

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

## Device Quick Reference

| Device                                               | IP                         | Role                 | Hardware                          |
| ---------------------------------------------------- | -------------------------- | -------------------- | --------------------------------- |
| wall-of-cards         | 192.168.1.1                  | Firewall/Router      | pfSense (N3700, 4x 2.5GbE)        |
| two-of-diamonds     | 192.168.1.2                  | Proxmox Host         | i7-8700K, 32GB RAM                |
| jack-of-diamonds   | 192.168.1.50                 | Primary DNS/Services | MeLE Mini PC (N5105, 8GB)         |
| king-of-diamonds   | 192.168.1.62                 | TrueNAS Storage      | Proxmox VM (48TB ZFS)             |
| six-of-spades         | 192.168.1.100                | Multi-Purpose Server | CTONE Mini PC (N95, 16GB)         |
| seven-of-spades     | 192.168.1.107                | Immich Photos        | Proxmox VM                        |
| three-of-diamonds | 192.168.1.200                | AI/ML Server         | i7-8700K, RTX 3090, 64GB          |
| queen-of-diamonds | 192.168.1.250                | Media Automation     | GMKtec Mini PC (N4100, 6GB)       |
| matrix                       | 10.0.1.120              | Matrix Homeserver    | Proxmox VM (DMZ)                  |
| ten-of-spades         | 192.168.1.10 / 172.16.1.10 | Gaming/Workstation   | i7-8700K, RTX 5070, 32GB          |
| joker-black             | 172.16.1.5               | Admin Workstation    | Dell 16 Plus (Core Ultra 9, 32GB) |

---

### Resources and Tools

| Tool<img width=182/>                                  | Description<img width=250/>           | Tutorials                                                                                                                                                                                                                  |
| ----------------------------------------------------- | ------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Tabby](https://github.com/Eugeny/tabby)              | Highly configurable terminal emulator | <img src="https://github.com/arm-ser/house-of-cards/blob/main/logos-screenshots/youtube.png?raw=true" width="15" /> [Tabby Tutorial](https://www.youtube.com/watch?v=-yfuYPowUDE)                                          |
| [Ventoy](https://www.ventoy.net/en/index.html)        | Easy Multiboot USB Creator            | <img src="https://github.com/arm-ser/house-of-cards/blob/main/logos-screenshots/youtube.png?raw=true" width="15" /> [Ventoy Tutorial](https://www.youtube.com/watch?v=-hs4mH7uBkk)                                         |
| [Obsidian](https://obsidian.md/)                      | One of the best note-taking tools     | <img src="https://github.com/arm-ser/house-of-cards/blob/main/logos-screenshots/youtube.png?raw=true" width="15" /> [Obsidian Tutorial Playlist](https://www.youtube.com/playlist?list=PL5fd4SsfvECy0zzf8Cyo20ZoipEt6YeL3) |
| [Lightshot](https://app.prntscr.com/en/download.html) | Convenient Screenshot taking tool     | -                                                                                                                                                                                                                          |

### Teachers 

<img src="https://github.com/arm-ser/house-of-cards/blob/main/logos-screenshots/youtube.png?raw=true" width="15" /> [Awesome Open Source](https://www.youtube.com/@AwesomeOpenSource/featured) <img src="https://github.com/arm-ser/house-of-cards/blob/main/logos-screenshots/youtube.png?raw=true" width="15" /> [Lawrence Systems](https://www.youtube.com/@LAWRENCESYSTEMS) <img src="https://github.com/arm-ser/house-of-cards/blob/main/logos-screenshots/youtube.png?raw=true" width="15" /> [DB Tech](https://www.youtube.com/@DBTechYT) <img src="https://github.com/arm-ser/house-of-cards/blob/main/logos-screenshots/youtube.png?raw=true" width="15" /> [Raid Owl](https://www.youtube.com/@RaidOwl) <img src="https://github.com/arm-ser/house-of-cards/blob/main/logos-screenshots/youtube.png?raw=true" width="15" /> [Ben Eater](https://www.youtube.com/@BenEater) <img src="https://github.com/arm-ser/house-of-cards/blob/main/logos-screenshots/youtube.png?raw=true" width="15" /> [IBRACORP](https://www.youtube.com/@IBRACORP) <img src="https://github.com/arm-ser/house-of-cards/blob/main/logos-screenshots/youtube.png?raw=true" width="15" /> [Techno Tim](https://www.youtube.com/@TechnoTim) <img src="https://github.com/arm-ser/house-of-cards/blob/main/logos-screenshots/youtube.png?raw=true" width="15" /> [Mark Furneaux](https://www.youtube.com/@TheUbuntuGuy) <img src="https://github.com/arm-ser/house-of-cards/blob/main/logos-screenshots/youtube.png?raw=true" width="15" /> [Christian Lempa](https://www.youtube.com/@christianlempa) <img src="https://github.com/arm-ser/house-of-cards/blob/main/logos-screenshots/youtube.png?raw=true" width="15" /> [John Hammond](https://www.youtube.com/@_JohnHammond)

<img src="https://github.com/arm-ser/house-of-cards/blob/main/logos-screenshots/youtube.png?raw=true" width="15" /> [Чёрный Треугольник](https://www.youtube.com/@Black_Triangle) <img src="https://github.com/arm-ser/house-of-cards/blob/main/logos-screenshots/youtube.png?raw=true" width="15" /> [DesignerMix](https://www.youtube.com/@DesignerMix) <img src="https://github.com/arm-ser/house-of-cards/blob/main/logos-screenshots/youtube.png?raw=true" width="15" /> [Нетипичный Безопасник](https://www.youtube.com/@MChannelone)  

<img src="https://github.com/arm-ser/house-of-cards/blob/main/logos-screenshots/linkedin.png?raw=true" width="15" /> [Sevada Isayan](https://www.linkedin.com/in/sevadaisayan/) <img src="https://github.com/arm-ser/house-of-cards/blob/main/logos-screenshots/linkedin.png?raw=true" width="15" /> [Gevorg Atoyan](https://www.linkedin.com/in/gevorgatoyan/)   
  
<img src="https://github.com/arm-ser/house-of-cards/blob/main/logos-screenshots/tumo.png?raw=true" width="15" /> [Tumo Center Of Creative Technologies](https://tumo.org/)     


##### Disclaimer
This home lab is intended strictly for educational and experimental purposes. Any use of information provided in this lab for illegal or unethical activities is strictly prohibited. The creator cannot be held responsible for any consequences resulting from misuse. By accessing and using information provided in this lab, you acknowledge and agree to comply with all applicable laws and ethical guidelines. Use this information responsibly and at your own risk.
