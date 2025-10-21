# 🧠 My Homelab Setup

## 💻 Hardware Overview
**Server Device:** Lenovo Laptop  
**CPU:** Intel Celeron N4000 (2 cores / 2 threads)  
**RAM:** 4 GB (2.5 GB free, 3.66 GB swap)  
**Storage:** 465.8 GB HDD  
**GPU:** Intel UHD Graphics 600 (for light desktop use)  
**OS:** Ubuntu  

> The setup runs on modest specs, so stability and resource management are key priorities. Docker and lightweight services are used to avoid overloading the system.

---

## 🐳 System & Containerization
The homelab is fully containerized using **Docker**, managed through **Portainer** (referred to as *Pochaina*).  
This setup allows flexible management, deployment, and scaling of services.

**Core Services Running:**
- **Docker** – Handles container orchestration  
- **Portainer** – Web-based Docker management UI  
- **Tailscale** – Enables secure remote access between server and client laptop  
- **Uptime Kuma** – Provides service uptime and performance monitoring  

---

## ☁️ Current Active Services

### **1. Nextcloud Server**
- Acts as a **personal cloud** for **backups, photos, videos, and contacts**.  
- Enables easy syncing and access from any device through Tailscale.  
- Serves as the base for future integrations like Google Calendar sync.  

### **2. Jellyfin Media Server**
- The main **media hub** of the setup, branded as **JetLeafian**.  
- Integrated with:  
  - **Sonarr** – for automated TV show downloads  
  - **Radarr** – for automated movie downloads  
  - **Lidarr** – for music automation  
  - **qBittorrent** – for torrent management  
- Fully automated fetching and organizing of **movies, shows, music, and books**.  
- Media is displayed and accessed through the Jellyfin UI.  

---

## 🔗 Networking & Access
- **Tailscale**: Securely connects the server and client devices without exposing ports.  
- **Remote Management**: All services accessible through private Tailscale IPs.  
- **Current Focus**: Strengthening the VPN and network segmentation for better isolation and performance.

---

## 📈 Monitoring
- **Uptime Kuma** is configured to track availability and response times of all containers.  
- Planned future metrics: container CPU/RAM monitoring and Docker health checks.

---

## 🧪 Learning & Experimentation Zone
A big part of this homelab is dedicated to **learning, experimentation, and testing**.

Planned and ongoing learning setups include:
- **Kali Linux Container** – for security testing, penetration practice, and “break and fix” learning.  
- **Cloud Engineering Lab** – environment for testing **AWS, Docker, and automation workflows**.  
- Used as a **sandbox** to safely experiment with new tools and configurations before deploying them to the main stack.

---

## 🔮 Future Plans
Planned additions to expand functionality and automation:

| Category | Planned Tool | Purpose |
|-----------|---------------|----------|
| **Media & Content** | **MeTube**, **PodGrab** | YouTube downloader and podcast fetcher |
| **Home Automation** | **Home Assistant** | Smart home management hub |
| **Monitoring & Analytics** | **Hammond** | Track and analyze service usage |
| **Productivity** | **Nextcloud + Google Calendar Integration** | Sync calendar and events |
| **Automation** | **Neon** | Automate workflows and container actions |
| **Utilities** | **Starlink PDF**, **IT Tools Suite** | Document conversion and tech utilities |

> The focus is to create a self-sustaining, private, and educational ecosystem that supports both personal use and continuous learning.

---

## 🧩 Summary
This homelab serves three main purposes:
1. **Personal Cloud & Media Hub** – Self-hosted storage and streaming.  
2. **Automation Playground** – Testing automated deployments with Docker.  
3. **Learning Environment** – A space to explore cybersecurity, cloud tech, and infrastructure management.  

It’s simple, efficient, and designed to grow alongside your technical learning journey.
