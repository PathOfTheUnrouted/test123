---
description: request any missing info to be added by emailing contact@pathoftheunrouted.org
---

# 🏭 Satisfactory Dedicated Server: Docker Deployment Guide (7-25)

![Satisfactory Logo](https://cdn.akamai.steamstatic.com/steam/apps/526870/header.jpg)

**Accurate port configuration** - Verified July 2024

## ✅ Requirements for this setup

<table><thead><tr><th>Component</th><th>Specification</th></tr></thead><tbody><tr><td><strong>OS</strong></td><td>Ubuntu Server 22.04+</td></tr><tr><td><strong>CPU</strong></td><td>4 cores (3.0GHz+)</td></tr><tr><td><strong>RAM</strong></td><td>8GB minimum (16GB recommended)</td></tr><tr><td><strong>Storage</strong></td><td>20GB+ SSD (saves grow rapidly)</td></tr><tr><td><strong>Ports</strong></td><td><pre><code><strong>7777/tcp+udp (Game), 8888/tcp (Reliable port)
</strong></code></pre></td></tr></tbody></table>

## 🚀 Deployment

### 1. Create Project Structure

```bash
mkdir -p ~/docker/satisfactory/{config,saves,logs}
cd ~/docker/satisfactory
```

### 2. Correct `docker-compose.yml` (2024)

```yaml
version: '3.8'
services:
  satisfactory:
    image: wolveix/satisfactory-server:latest
    container_name: satisfactory
    restart: unless-stopped
    ports:
      - "7777:7777/tcp"  # Primary game port (TCP)
      - "7777:7777/udp"  # Primary game port (UDP)
      - "8888:8888/tcp"  # Reliable Port
    volumes:
      - ./config:/config
      - ./saves:/home/steam/satisfactory/saved
      - ./logs:/var/log/satisfactory
    environment:
      - MAXPLAYERS=8
      - SERVERBEACONPORT=8888
      - SERVERGAMEPORT=7777
      - AUTOPAUSE=true  # Pauses when empty
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8888"]
      interval: 60s
      timeout: 5s
      retries: 3
```

## 🔧 Configuration

### Port Breakdown

| Port | Protocol | Purpose                  |
| ---- | -------- | ------------------------ |
| 7777 | TCP+UDP  | Game traffic (essential) |
| 8888 | TCP only | Reliable Port            |

### First-Time Setup

1.  **Start container:**

    ```bash
    docker compose up -d
    ```
2. **Claim Server:**\
   -Use the in game server manager and enter your server ip and port 7777\
   -the first person to connect will be prompted to set up the server\
   -Set Admin password, and load a save or create a new game
3. **Have fun!** (join button is the server manager window in the bottom right.)

### Update Server

```bash
docker compose pull && docker compose up -d --force-recreate
```

### Backup Saves

```bash
# Create timestamped backup
tar -czvf satisfactory-saves-$(date +%Y%m%d).tar.gz ./saves
```

### Key Commands

<table><thead><tr><th width="432">Command</th><th>Purpose</th></tr></thead><tbody><tr><td><pre><code>docker logs --tail 100 satisfactory 
</code></pre></td><td>View real-time logs (most recent 100)</td></tr><tr><td><pre><code>docker exec satisfactory supervisorctl status
</code></pre></td><td>Check service health</td></tr><tr><td><pre><code>tail -f ./logs/satisfactory.log
</code></pre></td><td>Monitor log file</td></tr></tbody></table>

## 🚨 Troubleshooting

<table><thead><tr><th>Symptom</th><th>Solution</th></tr></thead><tbody><tr><td>Can't connect via game client</td><td>Verify <code>7777/tcp+udp</code> open</td></tr><tr><td>Cannot connect to server API</td><td>Check <code>8888/tcp</code> firewall rules</td></tr><tr><td>"Server not responding"</td><td>Restart container + check CPU usage</td></tr><tr><td>Save corruption</td><td>Restore from backup in <code>./saves/backup/</code></td></tr><tr><td>Navmesh errors<img src=".gitbook/assets/image (1).png" alt=""></td><td><pre><code>docker exec -it satisfactory bash
cd /config/saved/server
rm -rf *.ue4navdata
exit
docker restart satisfactory
</code></pre></td></tr><tr><td>Desync issues &#x26; high cpu usage</td><td>Change the reliable port (8888) to one above 49152 (ephemeral ports)</td></tr></tbody></table>

***

**Current Specifications**:

*   Image:&#x20;

    ```
    wolveix/satisfactory-server:latest
    ```
*   Save Location:&#x20;

    ```
    ./saves/
    ```
*   Config Files:&#x20;

    ```
    ./config/
    ```
*   Logs:&#x20;

    ```
    ./logs/
    ```

All ports confirmed with Satisfactory 1.1&#x20;
