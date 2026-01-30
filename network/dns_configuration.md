# DNS Configuration

Network DNS configuration and local hostname mappings for the homelab.

## DNS Server

**Primary DNS**: Jack of Diamonds (192.168.1.50)
- **Service**: PiHole
- **Purpose**: Network-wide DNS with ad-blocking
- **Interface**: 53/UDP, 53/TCP
- **Upsteam DNS**: TBD

**Backup DNS**: TBD

## Local DNS Records (PiHole)

All devices use custom local DNS entries for easy access via short hostnames.

### Server Hostnames (A Records)

| Hostname    | IP Address    | Device          | Card Name         | Notes                |
| ----------- | ------------- | --------------- | ----------------- | -------------------- |
| jack.dmnd   | 192.168.1.50  | MeLE Mini PC    | Jack of Diamonds  | Primary DNS server   |
| six.spds    | 192.168.1.100 | CTONE Mini PC   | Six of Spades     | Multi-purpose server |
| three.dmnd  | 192.168.1.200 | Dell Precision  | Three of Diamonds | AI/ML workloads      |
| queen.dmnd  | 192.168.1.250 | GMKtec Mini PC  | Queen of Diamonds | Media automation     |
| king.dmnd   | 192.168.1.62  | TrueNAS VM      | King of Diamonds  | TrueNAS storage      |
| nas.local   | 192.168.1.62  | TrueNAS VM      | King of Diamonds  | Alias for TrueNAS    |
| ten.spds    | 172.16.1.50  | MINISFORUM GK41 | Ten of Spades     | Home Assistant       |


### Domain Aliases (Subdomains → homelab.myhomelabdomain.com)

The following subdomains point to `myhomelabdomain.com` (192.168.1.50):

| Subdomain | Service | Notes |
|-----------|---------|-------|
| cloud.myhomelabdomain.com | Nextcloud | File sync and collaboration |
| dashboard.myhomelabdomain.com | Dashboard | Main dashboard |
| dns.myhomelabdomain.com | PiHole | DNS management interface |
| home.myhomelabdomain.com | Homepage | Landing page |
| liveshare.myhomelabdomain.com | Liveshare | Code collaboration |
| portainer.myhomelabdomain.com | Portainer | Container management |
| warden.myhomelabdomain.com | Vaultwarden | Password manager |

### CNAME Records (Service Aliases)

CNAME records route service subdomains to their host devices:

**Services on homelab.myhomelabdomain.com (myhomelabdomain.com → 192.168.1.50):**
- ccdb, chat, diagram, fileshare, finances, firefox, firefox-sync, fxsync
- gateway, hook, image, importer, invoice, jackett, metube, music
- nas, nasportainer, obsidiansync, office, photo, proxy, qbit
- radarr, readarr, share, share.live, sonarr, sync, tabby
- temp, temp2, test, tubesync, uptime

**Services on six.spds (→ 192.168.1.100):**
- audiobooks, browser, claude, gallery, litellm, media, n8n
- request, rss, search, sosproxy, spellcheck

**Services on seven.dmnd (→ 192.168.1.51 - DECOMMISSIONED):**
- budget, pod

**Services on ten.spds (→ 172.16.1.50):**
- ha (Home Assistant)

**Services on queen.dmnd (→ queen-of-diamonds.com):**
- qodproxy

## Network Subnets

| Subnet | Purpose | VLAN | Gateway |
|--------|---------|------|---------|
| 192.168.1.0/24 | Main homelab network | Default/Untagged | 192.168.1.1 |
| 172.16.1.0/24 | Home Assistant network | TBD | TBD |

## DNS Resolution Flow

```
Client Device
    ↓
Jack of Diamonds (192.168.1.50) - PiHole
    ↓
[Check Local DNS Records]
    ↓ (if not local)
[Upstream DNS Server]
    ↓
Internet
```

## Related

- [Jack of Diamonds](../devices/jack_of_diamonds.md) - Primary DNS server device
- [Device Service Index](device_service_index.md)
- [Network Index](_index.md)

---

[← Back to Network](_index.md)
