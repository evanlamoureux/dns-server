# 🏡 Self-Hosted Infrastructure

A modular, environment-variable-driven infrastructure stack for running self-hosted services with Docker and Portainer. All services are organized by stack, run on a macvlan network, and support static IP addressing and secure TLS access via Caddy.

> **DO NOT COMMIT `.env` FILES** – All secrets and credentials live in `.env`, while `.env.template` provides configuration hints.

---

## 📁 Folder Structure

```
/etc/
├── dash/         → Heimdall  
├── dns/          → Pi-hole, Unbound, Exporters  
├── failover/     → Failover DNS infrastructure  
├── meal/         → Mealie recipe manager  
├── metrics/      → Grafana, Prometheus, Loki, Promtail, Node Exporter  
├── notes/        → Memos, PrivateBin  
├── portainer/    → Portainer  
├── proxy/        → Caddy reverse proxy  
├── sync/         → Nebula-Sync (Pi-hole replicator)  
├── update/       → Diun update notifier  
├── uptime/       → StatusOwl Dockman agent  
├── vault/        → Vaultwarden password manager  
└── vpn/          → Tailscale VPN  
```

Each stack includes:
- `STACK.yml` — Docker Compose file with variable references  
- `.env` — Secrets and config values (**not committed**)  
- `.env.template` — Public template with hints

---

## 🚀 Quickstart

```bash
# Clone this repo
git clone https://github.com/yourname/self-hosted-infra.git
cd self-hosted-infra

# Copy and configure the .env file for each stack
cp /etc/dns/.env.template /etc/dns/.env
micro /etc/dns/.env  # or nano, vim, etc.

# Deploy manually (or import into Portainer):
docker compose -f /etc/dns/dns.yml --env-file /etc/dns/.env up -d
```

---

## 📦 Stack Overview

| Stack         | Folder        | Services Included                          |
|---------------|---------------|--------------------------------------------|
| **Dashboard** | `dash/`       | Heimdall                                   |
| **DNS**       | `dns/`        | Pi-hole + Unbound + Exporters              |
| **Failover**  | `failover/`   | Redundant Pi-hole + Unbound + Exporters    |
| **Proxy**     | `proxy/`      | Caddy reverse proxy with local TLS         |
| **Metrics**   | `metrics/`    | Grafana, Prometheus, Loki, Promtail, Node Exporter |
| **Notes**     | `notes/`      | Memos, PrivateBin                          |
| **Vault**     | `vault/`      | Vaultwarden password manager               |
| **Updates**   | `update/`     | Diun container update notifier             |
| **Uptime**    | `uptime/`     | StatusOwl Dockman container monitor        |
| **VPN**       | `vpn/`        | Tailscale mesh VPN                         |
| **Sync**      | `sync/`       | Nebula-Sync for syncing Pi-hole instances  |

---

## 🛡️ Security Notes

- TLS certificates are issued internally by Caddy  
- Vaultwarden and other sensitive services are behind HTTPS and DNS-based access control  
- Tailscale provides secure remote access to the full network  
- Diun monitors for Docker image updates and can notify you externally  

---

## 📄 License

This repository’s **configuration files**, **templates**, and **documentation** are licensed under the [MIT License](LICENSE).

> ⚠️ This repository **orchestrates third-party software** (e.g., Grafana, Pi-hole, Vaultwarden), which are **licensed separately** by their respective maintainers.  
> Please refer to each application’s license for usage rights.
