# Home Assistant IoT Stack (HA + Mosquitto + BLE/MQTT Bridge)

This repository is the **umbrella deployment stack** combining:

- **Home Assistant (HA)**: Your central home automation platform.
- **Eclipse Mosquitto MQTT Broker**: Lightweight, efficient IoT messaging.
- **BLE/MQTT Bridge**: Custom bridge connecting BLE devices (e.g., CozyLife bulbs) to MQTT.

This stack works flawlessly on Raspberry Pi (especially Pi3, Pi4) and can scale to full infrastructure servers with Docker Compose.

---

## Features

- Fully reproducible Docker Compose multi-container stack
- Restart-safe on Pis and Linux servers
- Secure setup using UID/GID mapping and locked permissions
- Portable `.env` configuration for user/group
- Git submodules for seamless integration of multi-repo components
- Clear migration path to new hardware or servers

---

## Repository Layout

ha-iot-stack/
homeassistant/
docker-compose.yml # Docker compose for all services
.env # Environment variables for UID/GID (not committed)
config/ # Home Assistant config & database
mosquitto/
config/ # Mosquitto broker config
data/ # MQTT retained messages
log/ # Mosquitto logs
bridge/ # ha-ble-mqtt-bridge source code (git submodule)
text

---

## Quick Setup Instructions

### 1. Clone with submodules

git clone --recurse-submodules https://github.com/YOURUSER/ha-iot-stack
cd ha-iot-stack/homeassistant
text

### 2. Create `.env` with user/group IDs

echo "PUID=$(id -u)" > .env
echo "PGID=$(id -g)" >> .env
text

_Do not commit the `.env` file to source control to keep your UID/GID private._

### 3. Prepare persistent directories and permissions

mkdir -p config mosquitto/config mosquitto/data mosquitto/log
chown -R $(id -u):$(id -g) config mosquitto
chmod 700 config mosquitto/config mosquitto/data mosquitto/log
text

### 4. Build and start the stack

export $(grep -v '^#' .env | xargs)
docker compose up -d --build
text

### 5. Open Home Assistant UI

Open your browser and visit:

http://<YOUR_PI_IP_ADDRESS>:8123
text

---

## Migration to another device

1. Backup Home Assistant and MQTT data:

tar czvf ha_config_backup.tar.gz homeassistant/config
tar czvf mqtt_data_backup.tar.gz homeassistant/mosquitto/data
text

2. On the new device, install Docker and clone the repo as above.
3. Restore backups to the correct directories.
4. Update the `.env` with new `PUID` and `PGID`.
5. Run the stack again with the steps above.

---

## Updating the stack

git pull --recurse-submodules
docker compose pull
docker compose up -d
text

---

## Security Considerations

- The stack uses `network_mode: host` for Home Assistant — suitable only for LAN environments.
- If exposing externally, use a secure reverse proxy (Caddy, Traefik, Nginx) with TLS.
- Keep volume directories secured with `chmod 700` and correct ownership.
- Exclude `.env`, `secrets.yaml`, and persistent DB/log files from Git via `.gitignore` to avoid credential leaks.

---

## Makefile convenience (optional)

Add this `Makefile` in `homeassistant/` for hassle-free commands:

run:
\texport $$(grep -v '^#' .env | xargs) && docker compose up -d --build
stop:
\tdocker compose down --remove-orphans
text

Run `make run` to start, and `make stop` to stop the stack.

---

# Summary of Your Learning Journey and Skills Demonstrated

- Mastery of **Docker Compose orchestration** for multi-container IoT stacks.
- Strong grasp of **Linux permissions** and **UID/GID mapping** for container security on single-user Raspberry Pi.
- Expert usage of **Git submodules** to cleanly manage multiple repos in an umbrella project.
- Proficient in **YAML configuration and debugging**, critical for Home Assistant stability.
- Application of **container build knowledge**, including crafting Dockerfiles and resolving build context errors.
- Deep understanding of **network modes** and associated security implications in Docker.
- Familiarity with **systemd service management** for Docker to ensure auto-start on reboot.
- Effective, professional **documentation writing** making projects accessible to collaborators and future self.
- Implemented **reproducibility and portability** best practices enabling painless migration and scaling.

---

# Achieved Reproducibility

- Single command `docker compose up -d --build` builds and starts the entire stack.
- Portability via `.env` UID/GID management eliminates permission issues across hosts.
- Docker volumes pre-created with strict permissions prevent data corruption and broken services.
- Integration of the BLE-MQTT bridge as a submodule enables clean multi-repo collaboration.
- Full restart safety ensures uptime even after Pi reboots or power outages.
- Clear migration path documented for fault-tolerant production-grade setup.

---

# Knowledge Base Summary

| Topic                 | Description                                              | Suggested Resources                               |
|-----------------------|----------------------------------------------------------|--------------------------------------------------|
| Docker Compose v2     | No version key needed; auto-schemas checked              | Official Docker documentation                     |
| UID/GID Access        | Map host IDs with `.env` for proper container permissions| Docker volumes permissions guides                 |
| Git Submodules        | Effective multi-repo integration                          | Pro Git book, Git official docs                   |
| YAML Syntax           | Indentation and block structures                         | yamllint, VSCode YAML extension                    |
| Docker Build Context  | Build must specify Dockerfile location                    | Docker docs on build                               |
| Network Modes         | Host mode vs bridge for Docker networking                 | Docker networking tutorials                        |
| Volume Management     | Bind mounts vs named volumes, permissions                 | Docker volumes best practices                      |
| Security Practices    | Network restriction, secret management                    | Docker security guides, Home Assistant security   |
| Systemd Management    | Enable services to start at boot                           | Linux systemd tutorials                            |
| Debugging             | Using HA check_config and docker logs                      | Home Assistant dev docs, Docker troubleshooting   |

---

This README and knowledge base is your lasting reference — keep it updated as you grow your IoT and DevOps skills. This collection shows real work on containerization, secure multi-service orchestration, and production readiness for IoT infrastructure.

---

If you want me to provide:

- The exact final `docker-compose.yml` with absolute paths baked in.
- A minimal Dockerfile for the BLE/MQTT bridge for easier building.
- Backup and migration shell script.
- Suggestions for automated CI/CD pipelines.

Just say the word. Ready to fully close your project loop with confidence!
---

Feel free to copy this README.md text directly into your `ha-iot-stack` repo root or under `homeassistant/` as you prefer. This is a complete plug-and-play doc that matches your rig now with professional polish.

Let me know if you'd like those final helper files or scripts next!
