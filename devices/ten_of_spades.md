# Ten of Spades (MINISFORUM GK41)

## Notes

- Home Assistant OS installation for home automation
- Manages smart home devices, automations, and integrations
- Homebridge add-on for Apple HomeKit integration
- Separate network (172.16.1.0/24) for Home Assistant
- All services local-only, no internet exposure
- Low power consumption for 24/7 operation

## Device Information

**Card Name**: Ten of Spades
**Hardware**: MINISFORUM GK41 (BESSTAR TECH LIMITED)
**CPU**: Intel (TBD)
**RAM**: TBD
**Storage**: TBD
**Network**: 1Gbps Ethernet

**Management Interface**: SSH (port 22), Web UI (port 9001)

## Network Configuration

**IP Address**: 172.16.1.50
**Hostname**: ten.spds
**DNS**: 192.168.1.50

## Operating System

**OS**: Home Assistant OS (Debian-based)

## Running Services

### Docker Containers

| Container Name              | Purpose                                  | Ports |
| --------------------------- | ---------------------------------------- | ----- |
| homeassistant               | Home Assistant core                      | 9001  |
| hassio_supervisor           | Home Assistant supervisor                | -     |
| hassio_cli                  | Home Assistant CLI                       | -     |
| hassio_dns                  | Home Assistant DNS                       | -     |
| hassio_audio                | Home Assistant audio                     | -     |
| hassio_multicast            | Home Assistant multicast/mDNS            | -     |
| hassio_observer             | Home Assistant observer                  | 9002  |
| addon_core_configurator     | File editor add-on                       | -     |
| addon_core_matter_server    | Matter smart home protocol server        | -     |
| homebridge                  | HomeKit bridge for Apple Home            | -     |

### Exposed Ports

| Port | Service                    |
| ---- | -------------------------- |
| 22   | SSH                        |
| 9001 | Home Assistant Web UI      |
| 9002 | Home Assistant Observer    |

## Related

- [Device Service Index](device_service_index.md)
- [Hardware Specs](../hardware/device_hardware_index.md)

---

[← Back to Device Index](device_service_index.md)
