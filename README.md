# cloudflare-ddns

**Location:** `/srv/docker/cloudflare-ddns/` on mbuntu (192.168.1.35)

Unified Cloudflare DDNS updater for all `am180.us` subdomains. Polls the WAN IP
every 5 minutes and updates DNS A records in Cloudflare automatically.

Consolidated from two separate containers (Mac Pro + mbuntu) to a single container
on mbuntu in v2.0.0 (2026-02-21).

---

## Service

| Container | Image | Mode | Purpose |
|---|---|---|---|
| `cloudflare-ddns-mbuntu` | favonia/cloudflare-ddns | `network_mode: host` | WAN IP to Cloudflare DNS |

Uses `network_mode: host` so it detects the true WAN IP without NAT confusion.

---

## Managed Domains

All subdomains of `am180.us` (see `docker-compose.yml` `DOMAINS` env var):
`sonarr`, `radarr`, `prowlarr`, `overseerr`, `qbit`, `bitmagnet`, `jackett`,
`mail`, `grafana`, `prometheus`, `alertmanager`, `adguard`, `lidarr`, `stack`.

---

## Quick Start

```bash
cd /srv/docker/cloudflare-ddns
docker compose up -d
docker compose logs -f --tail=50
docker compose down && docker compose up -d
```

---

## Credentials

Loaded from `/srv/docker/secrets/cloudflare.env` via `env_file`. Host-only — NOT committed.
Contains `CF_API_TOKEN`. The `cloudflare.env` file in this directory is a local pointer/alias;
the authoritative copy is in `/srv/docker/secrets/`.

---

## Security

Container runs with all capabilities dropped, read-only filesystem, no-new-privileges.
