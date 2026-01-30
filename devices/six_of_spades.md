# Six of Spades (CTONE Mini PC)

## Notes

- Multi-purpose server running diverse self-hosted applications
- Hosts AI/LLM services (LiteLLM proxy, Open WebUI, pipelines)
- Runs productivity tools (n8n automation, CommaFeed RSS, LanguageTool)
- Provides media services (Jellyfin, Audiobookshelf, Navidrome)
- Network utilities (SearXNG search, Cloudflared tunnel, Nginx Proxy Manager)
- VPN routing via Gluetun container
- All services local-only, no direct internet exposure
- 20W power consumption

## Device Information

**Card Name**: Six of Spades
**Hardware**: CTONE Mini PC
**CPU**: Intel N95 (up to 3.4GHz)
**RAM**: 16GB
**Storage**: 512GB M.2
**Power Draw**: 20W
**Network**: 1GbE

**Management Interface**: SSH (port 22)

## Network Configuration

**IP Address**: 192.168.1.100
**Hostname**: six.spds
**DNS**: 192.168.1.50

## Operating System

**OS**: Ubuntu 22.04.5 LTS

## Running Services

### Docker Containers

| Container Name      | Purpose                              | Ports       |
| ------------------- | ------------------------------------ | ----------- |
| n8n_mysql           | MySQL database for n8n               | -           |
| finance-dashboard   | Finance dashboard (nginx)            | 9001        |
| n8n-redis           | Redis cache for n8n                  | -           |
| npm-app             | Nginx Proxy Manager                  | 80, 81, 443 |
| npm-db              | NPM MariaDB database                 | -           |
| jellyfin-youtube    | Jellyfin instance for YouTube        | 9002        |
| jellyfin            | Media server for movies/TV shows     | 9003        |
| cloudflared         | Cloudflare tunnel client             | -           |
| commafeed           | Self-hosted RSS feed reader          | 9004        |
| gluetun             | VPN client routing container traffic | -           |
| n8n                 | Workflow automation platform         | 9005        |
| n8n-postgres        | PostgreSQL database for n8n          | -           |
| rssbrew_redis       | Redis cache for RssBrew              | -           |
| rssbrew             | RSS to webhook/automation            | 9007        |
| baserow             | No-code database platform            | 9008        |
| watchtower          | Container auto-updater               | -           |
| protonmail-bridge   | ProtonMail IMAP/SMTP bridge          | 9009        |
| portainer-agent     | Remote container management agent    | 9010        |

**Note**: Some services route through gluetun VPN container.

### Exposed Ports

| Port | Service               |
| ---- | --------------------- |
| 22   | SSH                   |
| 80   | HTTP (NPM)            |
| 81   | NPM Admin             |
| 443  | HTTPS (NPM)           |
| 9001 | Finance Dashboard     |
| 9002 | Jellyfin YouTube      |
| 9003 | Jellyfin              |
| 9004 | CommaFeed             |
| 9005 | n8n Web UI            |
| 9007 | RssBrew               |
| 9008 | Baserow               |
| 9009 | ProtonMail Bridge     |
| 9010 | Portainer Agent       |

## Related

- [Device Service Index](device_service_index.md)
- [Hardware Specs](../hardware/device_hardware_index.md)

---

[← Back to Device Index](device_service_index.md)
