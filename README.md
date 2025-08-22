# 🏠 Homelab Documentation - Phase 1: Foundation

## 📋 Overview

This repository documents my homelab journey, starting with the essential foundation components. Phase 1 establishes the core infrastructure needed for all future expansions.

## 🎯 Phase 1: Foundation Components

### ✅ Completed Infrastructure

- **Ubuntu Server (Minimal)** - Base operating system
- **Docker + Portainer** - Container management platform
- **Tailscale** - Secure remote access VPN

## 🖥️ Hardware Specifications

- **Server**: Lenovo laptop  
  - CPU: Intel Celeron N4000, 2 cores / 2 threads  
  - RAM: 4 GB (2.5 GB free, 3.66 GB swap)  
  - Storage: 465.8 GB HDD  
  - GPU: Intel UHD 600 (for light desktop use only)

- **Network**:  
  - Using phone hotspot for internet  
  - Tailscale VPN for remote access between devices

- **Additional Hardware**:  
  - Windows client laptop for remote management  
  - Optional peripherals (monitor, keyboard, etc.) as needed


## 🛠️ Installation Guide

### 1. Ubuntu Server Setup

```bash
# Initial system update
sudo apt update && sudo apt upgrade -y

# Install essential packages
sudo apt install -y curl wget git htop neofetch
```

### 2. Docker Installation

```bash
# Remove old Docker versions
sudo apt-get remove docker docker-engine docker.io containerd runc

# Install Docker's official GPG key
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg lsb-release
sudo mkdir -m 0755 -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

# Add Docker repository
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker Engine
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Add user to docker group
sudo usermod -aG docker $USER

# Start and enable Docker
sudo systemctl enable docker
sudo systemctl start docker
```

### 3. Portainer Installation

```bash
# Create Portainer volume
docker volume create portainer_data

# Run Portainer container
docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart=always \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v portainer_data:/data \
  portainer/portainer-ce:latest
```

**Access Portainer**: `https://your-server-ip:9443`

### 4. Tailscale Installation

```bash
# Add Tailscale's package signing key and repository
curl -fsSL https://pkgs.tailscale.com/stable/ubuntu/jammy.noarmor.gpg | sudo tee /usr/share/keyrings/tailscale-archive-keyring.gpg >/dev/null
curl -fsSL https://pkgs.tailscale.com/stable/ubuntu/jammy.list | sudo tee /etc/apt/sources.list.d/tailscale.list

# Install Tailscale
sudo apt update
sudo apt install tailscale

# Connect to your Tailscale network
sudo tailscale up

# Enable IP forwarding for subnet routing (optional)
echo 'net.ipv4.ip_forward = 1' | sudo tee -a /etc/sysctl.conf
echo 'net.ipv6.conf.all.forwarding = 1' | sudo tee -a /etc/sysctl.conf
sudo sysctl -p /etc/sysctl.conf
```

## 🔧 Configuration

### Docker Compose Setup

Create a `docker-compose.yml` file for managing services:

```yaml
version: '3.8'

services:
  portainer:
    image: portainer/portainer-ce:latest
    container_name: portainer
    restart: unless-stopped
    ports:
      - "8000:8000"
      - "9443:9443"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - portainer_data:/data

volumes:
  portainer_data:
```

### Firewall Configuration

```bash
# Enable UFW
sudo ufw enable

# Allow SSH
sudo ufw allow ssh

# Allow Portainer
sudo ufw allow 9443/tcp
sudo ufw allow 8000/tcp

# Allow Tailscale
sudo ufw allow 41641/udp
```

## 🌐 Network Access

### Local Access
- **Portainer**: `https://[SERVER-IP]:9443`
- **SSH**: `ssh username@[SERVER-IP]`

### Remote Access (via Tailscale)
- **Portainer**: `https://[TAILSCALE-IP]:9443`
- **SSH**: `ssh username@[TAILSCALE-IP]`

## 🔍 Verification & Testing

### Verify Docker Installation
```bash
# Check Docker version
docker --version

# Test Docker
docker run hello-world
```

### Verify Portainer
1. Navigate to `https://your-server-ip:9443`
2. Create admin account
3. Connect to local Docker environment
4. Verify container visibility

### Verify Tailscale
```bash
# Check Tailscale status
sudo tailscale status

# Test connectivity from remote device
ping [TAILSCALE-IP]
```

## 🚨 Troubleshooting

### Common Issues

**Docker Permission Denied**
```bash
# Re-login after adding user to docker group
exit
# SSH back in, or run:
newgrp docker
```

**Portainer SSL Certificate Warning**
- This is expected on first run
- Click "Advanced" → "Proceed to [IP] (unsafe)"
- Consider setting up proper SSL certificates later

**Tailscale Connection Issues**
```bash
# Restart Tailscale
sudo systemctl restart tailscaled
sudo tailscale up
```
## 📚 Resources

- [Docker Documentation](https://docs.docker.com/)
- [Portainer Documentation](https://docs.portainer.io/)
- [Tailscale Documentation](https://tailscale.com/kb/)
- [Ubuntu Server Guide](https://ubuntu.com/server/docs)

## 🤝 Contributing

This is a personal homelab documentation project, but feel free to:
- Report issues or suggest improvements
- Share your own homelab experiences
- Contribute to the documentation

## 📄 License

This documentation is released under the MIT License.

---

**Last Updated**: [Date]  
**Status**: ✅ Phase 1 Complete
