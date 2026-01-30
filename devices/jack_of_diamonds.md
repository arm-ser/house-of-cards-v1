# Jack of Diamonds (MeLE Mini PC)

## Notes

- Primary services host running 15 Docker containers
- Serves as network DNS through PiHole (all devices point to 192.168.1.50)
- Nginx Proxy Manager handles reverse proxy for all services
- Low power consumption (10W) makes it ideal for 24/7 operation
- All services are local network only - no external exposure
  
## Device Information

**Card Name**: Jack of Diamonds
**Hardware**: MeLE Mini PC
**CPU**: Celeron 11th Gen N5105 (Up to 2.9GHz)
**RAM**: 8GB
**Storage**: 256GB M.2
**Power Draw**: 10W
**Network**: 1GbE

**Management Interface**: SSH (port 22)
## Network Configuration

**IP Address**: 192.168.1.50
**Hostname**: jack.dmnd (also myhomelabdomain.com)
**DNS**: 192.168.1.50 (self/PiHole)
## Operating System

**OS**: Ubuntu 22.04.5 LTS
## Running Services

### Docker Containers

| Container Name         | Purpose                        | Ports       |
| ---------------------- | ------------------------------ | ----------- |
| backrest               | Backup management              | 9001        |
| firefox_syncstorage    | Firefox Sync storage           | 9002        |
| firefox_syncstorage_db | Firefox Sync MariaDB           | -           |
| firefox_tokenserver_db | Firefox Token MariaDB          | -           |
| homepage               | Dashboard/homepage             | -           |
| firefly_iii_cron       | Firefly III cron jobs          | -           |
| firefly_iii_core       | Personal finance manager       | -           |
| firefly_iii_db         | Firefly III database           | -           |
| bitwarden              | Password manager (Vaultwarden) | -           |
| ghostboard             | Ghostboard server              | -           |
| nextcloud              | Cloud storage platform         | -           |
| nextcloud-db           | Nextcloud MariaDB              | -           |
| uptime-kuma            | Uptime monitoring              | 9003        |
| onlyoffice             | Office document server         | 9004        |
| hedgedoc-app           | Collaborative markdown editor  | -           |
| hedgedoc-db            | HedgeDoc PostgreSQL            | -           |
| npm-app                | Nginx Proxy Manager            | 80, 81, 443 |
| npm-db                 | NPM MariaDB                    | -           |
| pihole                 | DNS ad-blocker (with Unbound)  | 53, 9005    |
| portainer              | Container management           | 9006        |

### Exposed Ports

| Port | Service              |
| ---- | -------------------- |
| 22   | SSH                  |
| 53   | DNS (PiHole)         |
| 80   | HTTP (NPM)           |
| 81   | NPM Admin            |
| 443  | HTTPS (NPM)          |
| 9003 | Uptime Kuma          |
| 9002 | Firefox Sync Storage |
| 9005 | PiHole Web UI        |
| 9004 | OnlyOffice           |
| 9006 | Portainer            |
| 9001 | Backrest             |

## Related

- [Device Service Index](device_service_index.md)
- [Hardware Specs](../hardware/device_hardware_index.md)

---

[← Back to Device Index](device_service_index.md)
