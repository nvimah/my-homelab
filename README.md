## 🏠 My Personal Homelab

![Status](https://img.shields.io/badge/status-active-success)
![OS](https://img.shields.io/badge/OS-Ubuntu-blue)
![Docker](https://img.shields.io/badge/Containerization-Docker-informational)
![Automation](https://img.shields.io/badge/Automation-Enabled-brightgreen)
![Learning](https://img.shields.io/badge/Focus-Cloud%20%7C%20Security%20%7C%20Automation-yellow)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-orange.svg)](./CONTRIBUTING.md)
[![License](https://img.shields.io/badge/license-See%20LICENSE-lightgrey)](./LICENSE)

> A personal self-hosted environment built for automation, media management, cloud learning, and cybersecurity experimentation.

---

### 📚 Table of Contents

- **[Hardware Overview](#️-hardware-overview)**
- **[Current Setup](#-current-setup)**
  - **[Core Services](#core-services)**
- **[Learning & Experimentation](#-learning--experimentation-environment)**
- **[Architecture](#-architecture)**
- **[Repository Structure](#-repository-structure)**
- **[Getting Started](#-getting-started)**
  - **[Prerequisites](#prerequisites)**
  - **[Quick Start](#quick-start)**
  - **[Environment Variables](#environment-variables)**
- **[Operations](#-operations)**
  - **[Security Notes](#security-notes)**
  - **[Backups](#backups)**
  - **[Monitoring](#monitoring)**
- **[Future Plans](#-future-plans)**
- **[Screenshots](#-screenshots)**
- **[Author](#-author)**

---

### 🖥️ Hardware Overview

| Component | Specification |
|------------|---------------|
| **Device** | Lenovo Laptop |
| **CPU** | Intel Celeron N4000 (2 cores / 2 threads) |
| **RAM** | 4 GB (2.5 GB usable, 3.66 GB swap) |
| **Storage** | 465.8 GB HDD |
| **GPU** | Intel UHD 600 (for light desktop use) |

⚙️ *The system runs efficiently on modest hardware, focusing on lightweight containers and automation.*

---

### ⚙️ Current Setup

- **OS:** Ubuntu
- **Containerization:** Docker (managed via Portainer)
- **Remote Access:** Tailscale
- **Monitoring:** Uptime Kuma

#### Core Services

- **🗂️ Nextcloud** — Personal cloud
  - File backups, photo/video storage, contacts
- **🎬 Jellyfin** — Media hub
  - Integrated with:
    - **Sonarr** (TV)
    - **Radarr** (Movies)
    - **Lidarr** (Music)
    - **qBittorrent** (Torrent client)
  - Fully automated fetching, organizing, and serving media
  - Connected with **Ordinary App** for enhanced UI and access
- **🛠 Portainer** — Docker GUI management
- **📈 Uptime Kuma** — Uptime and status monitoring
- **🔐 Tailscale** — Secure remote access (no port forwarding)

---

### 🧠 Learning & Experimentation Environment

This lab doubles as a **hands-on learning platform**.

- **🔐 Kali Linux Container**
  - Sandbox for security testing, ethical hacking, and network troubleshooting
  - Used to safely break, test, and fix configurations
  - Supports real-world cybersecurity training

- **☁️ Cloud Engineering Practice**
  - Practicing AWS and hybrid setups locally
  - Testing deployments, container networking, and automation scripts
  - Experimenting with CI/CD concepts using lightweight infrastructure

🧪 *Goal: learn by building, breaking, and rebuilding.*

---

### 🧱 Architecture

```text
Clients (LAN/WAN)
   │
   └──▶ Tailscale (mesh VPN)
              │
              ├──▶ Portainer (manage containers)
              ├──▶ Nextcloud (private cloud)
              ├──▶ Jellyfin (media server)
              │       ├── Sonarr / Radarr / Lidarr
              │       └── qBittorrent
              └──▶ Uptime Kuma (status/monitoring)

Storage volumes (bind mounts) ↔ Services
Backups (local + remote/drive) ← scheduled jobs
```

- **Design goals:** automation-first, low-resource footprint, secure-by-default (Tailscale), and simple operations.

---

### 📂 Repository Structure

```text
.
├── assets/                       # Images, diagrams, screenshots
│   ├── architecture.png
│   ├── portainer-dashboard.png
│   ├── jellyfin-ui.png
│   ├── nextcloud-ui.png
│   └── uptime-kuma.png
├── compose/                      # (Optional) split Docker Compose stacks
│   ├── core.yml                  # Portainer, Uptime Kuma, Tailscale
│   ├── cloud.yml                 # Nextcloud + DB
│   └── media.yml                 # Jellyfin + Sonarr + Radarr + Lidarr + qBittorrent
├── services/                     # Per-service config and data mounts
│   ├── nextcloud/
│   ├── jellyfin/
│   ├── qbittorrent/
│   ├── sonarr/
│   ├── radarr/
│   ├── lidarr/
│   └── uptime-kuma/
├── scripts/                      # Helper scripts (backup, restore, maintenance)
│   ├── backup.sh
│   └── restore.sh
├── .env.example                  # Template for environment variables
├── docker-compose.yml            # Single file compose (alternative to ./compose/*.yml)
├── LICENSE                       # Project license (choose yours)
└── README.md                     # You are here
```

> Note: This structure is designed for clarity and portability on low-spec hardware. Use either `docker-compose.yml` or the split files under `compose/`.

---

### 🚀 Getting Started

#### Prerequisites

- Ubuntu host with basic shell access
- Docker Engine and Docker Compose plugin
- Tailscale account (for remote access)
- Optional: a domain and reverse proxy (Caddy/Traefik) if exposing services

#### Quick Start

```bash
# 1) Clone
git clone <your-repo-url> homelab && cd homelab

# 2) Configure
cp .env.example .env
# Edit .env with your timezone, PUID/PGID, storage paths, etc.

# 3) Bring up core services
# Option A: single compose file
docker compose up -d

# Option B: split stacks (example)
# docker compose -f compose/core.yml -f compose/cloud.yml -f compose/media.yml up -d

# 4) Join Tailscale (if using containerized tailscale)
# docker exec -it tailscale tailscale up --authkey=${TAILSCALE_AUTHKEY}

# 5) Access UIs (ports may vary by compose config)
# - Portainer:     https://<HOST>:9443
# - Nextcloud:     http://<HOST>:8081
# - Jellyfin:      http://<HOST>:8096
# - Sonarr:        http://<HOST>:8989
# - Radarr:        http://<HOST>:7878
# - Lidarr:        http://<HOST>:8686
# - qBittorrent:   http://<HOST>:8080
# - Uptime Kuma:   http://<HOST>:3001
```

#### Environment Variables

```dotenv
# Generic
TZ="Etc/UTC"
PUID=1000
PGID=1000

# Tailscale (if containerized)
TAILSCALE_AUTHKEY="tskey-xxxxxxxxxxxxxxxxxxxx"
TAILSCALE_HOSTNAME="homelab-node"

# Nextcloud
NEXTCLOUD_URL="http://nextcloud:80"
NEXTCLOUD_TRUSTED_DOMAINS="nextcloud,localhost"
NEXTCLOUD_ADMIN_USER="admin"
NEXTCLOUD_ADMIN_PASSWORD="change-me"

# Media Stack
JELLYFIN_HTTP_PORT=8096
QBITTORRENT_WEBUI_PORT=8080
SONARR_PORT=8989
RADARR_PORT=7878
LIDARR_PORT=8686

# Paths (adjust to your disks)
DATA_ROOT="/srv/homelab"
MEDIA_ROOT="${DATA_ROOT}/media"
DOWNLOADS_ROOT="${DATA_ROOT}/downloads"
BACKUPS_ROOT="${DATA_ROOT}/backups"
```

> Tip: Keep `.env` out of version control if it contains secrets.

---

### 🛠 Operations

#### Security Notes

- Prefer **Tailscale** for remote access; avoid exposing services to the public Internet.
- Create non-root Docker users with stable `PUID`/`PGID` for file permissions.
- Store credentials outside of the repo (e.g., using Docker secrets or environment files not committed).
- Keep images updated (consider **Watchtower** or scheduled updates via Portainer).

#### Backups

- Use `scripts/backup.sh` to snapshot critical volumes (e.g., `services/nextcloud`, databases, configs) to `BACKUPS_ROOT`.
- Optionally mirror backups to cloud (rclone/restic) for offsite protection.
- Test restores periodically with `scripts/restore.sh`.

#### Monitoring

- **Uptime Kuma** monitors internal endpoints (Jellyfin, Nextcloud, Portainer, etc.).
- Optionally add resource monitoring (node-exporter + Prometheus + Grafana) if resources allow.

---

### 🚧 Future Plans

| Tool | Description |
|------|--------------|
| **MeTube** | Browser-based video downloader for tutorials and learning content |
| **PodGrab** | Podcast downloader with library integration |
| **Home Assistant** | Smart home and IoT automation experiments |
| **Hammond** | Self-hosted bookmark and knowledge management |
| **Google Calendar Integration** | Sync tasks and events with Nextcloud |
| **Neon** | Workflow automation and container orchestration |
| **Starlink PDF** | PDF and document automation system |
| **IT Tools Suite** | Networking and sysadmin utilities dashboard |

Each addition is aimed at enhancing **automation**, **self-sufficiency**, and **technical learning**.

---

### 🧩 Vision Summary

This homelab currently serves as a:
- **Private cloud** (Nextcloud)
- **Automated media hub** (Jellyfin stack)
- **Secure remote lab** (Tailscale + Kali)
- **Monitoring and uptime system** (Uptime Kuma)

Planned expansions will transform it into a **complete self-hosted tech ecosystem**:
- Automation-first
- Cloud-integrated
- Security-aware
- Learning-driven

💡 *A sandbox for exploring everything tech — from Docker to DevOps to cybersecurity.*

---

### 🖼️ Screenshots

_Add screenshots or terminal previews here._

| Description | Screenshot |
|--------------|-------------|
| Portainer Dashboard | ![Portainer](assets/portainer-dashboard.png) |
| Jellyfin Media UI | ![Jellyfin](assets/jellyfin-ui.png) |
| Nextcloud Web Interface | ![Nextcloud](assets/nextcloud-ui.png) |
| Uptime Kuma Monitoring | ![UptimeKuma](assets/uptime-kuma.png) |

> Optional: add an `assets/architecture.png` diagram referencing the [Architecture](#-architecture) section.

---

### 🧑‍💻 Author

**Sharon Wainaina**  
Tech Enthusiast | Cloud Learner | Automation Builder  
💬 Always experimenting. Always learning.

---

### 📝 License

Choose a license appropriate for your goals (e.g., MIT for permissive, AGPL for copyleft, or keep private). Add it as `LICENSE` in the repo root.

---

### 🙌 Acknowledgements

- Docker community and maintainers of self-hosted apps
- Jellyfin, Sonarr, Radarr, Lidarr, Nextcloud, Portainer, Uptime Kuma, and Tailscale projects

---

> If you find this useful or have ideas, feel free to open an issue or discussion!
