# Queen of Diamonds (GMKtec Mini PC)

## Notes

- Media automation and download server
- Requests and downloads media content, organizes it on NAS server
- Runs automated media management pipeline (indexing, downloading, organizing)
- All services local-only, no internet exposure
- 10W power consumption
- Standard docker data directory: /home/homelab/docker/[service-name]
## Device Information

**Card Name**: Queen of Diamonds
**Hardware**: GMKtec Mini PC
**CPU**: Intel Celeron N4100 (up to 2.4GHz)
**RAM**: 6GB DDR4
**Storage**: 128GB eMMC
**Power Draw**: 10W
**Network**: 1Gbps Ethernet

**Management Interface**: SSH (port 22)

## Network Configuration

**IP Address**: 192.168.1.250
**Hostname**: queen.dmnd
**DNS**: 192.168.1.50

## Operating System

**OS**: Ubuntu 22.04.5 LTS

## Running Services

### Docker Containers

| Container Name  | Purpose                             | Ports |
| --------------- | ----------------------------------- | ----- |
| SearXNG         | Privacy-respecting metasearch       | -     |
| SearXNG-Redis   | Redis cache for SearXNG             | -     |
| pinchflat       | YouTube channel archiver            | 9001  |
| metube          | YouTube video downloader            | 9002  |
| proton-ovpn     | ProtonVPN container (gluetun)       | -     |
| radarr          | Movie management/automation         | 9003  |
| jellyseerr      | Media request management            | 9004  |
| jackett         | Torrent/Usenet indexer proxy        | 9005  |
| flaresolverr    | Cloudflare bypass for scrapers      | 9006  |
| sonarr          | TV show management/automation       | 9007  |
| qbit            | qBittorrent download client         | 9008  |
| protonvpn       | ProtonVPN container (gluetun)       | -     |
| portainer-agent | Remote container management agent   | 9010  |

**Note**: Most services route through VPN containers (protonvpn/proton-ovpn) for privacy.

### Exposed Ports

| Port | Service         |
| ---- | --------------- |
| 22   | SSH             |
| 9001 | Pinchflat       |
| 9002 | Metube          |
| 9003 | Radarr          |
| 9004 | Jellyseerr      |
| 9005 | Jackett         |
| 9006 | FlareSolverr    |
| 9007 | Sonarr          |
| 9008 | qBittorrent     |
| 9010 | Portainer Agent |

## Related

- [Device Service Index](device_service_index.md)
- [Hardware Specs](../hardware/device_hardware_index.md)

---

[← Back to Device Index](device_service_index.md)
