# AI-Accelerated DevSecOps Home Lab

This repository contains the core Docker Compose stack for a multi-layered, secure, and resilient smart home lab. This project demonstrates a practical application of DevOps principles, IoT integration, and network security to create a stable and extensible home automation ecosystem.

For the full story behind this project, see [JOURNEY.md](JOURNEY.md).
For setup and installation instructions, see [INSTALL.md](INSTALL.md).

## Core Components

* **`ha-iot-stack` (This Repository):** The central orchestration layer using Docker Compose to run Home Assistant, an MQTT Broker, and custom middleware.
* **`ha-ble-mqtt-bridge` (Custom Middleware):** A custom Python service that reverse-engineers and controls a proprietary Bluetooth LE device, bridging it to the MQTT network.
* **`rpispy-tailscale-autoheal` (Network Resilience):** A self-healing system on a Raspberry Pi that ensures 24/7 uptime for the lab's secure Tailscale mesh VPN.

## Key Skills Demonstrated

* **DevOps & Automation**: CI/CD (GitHub Actions), Containerization (Docker), Infrastructure as Code (IaC).
* **Security (SecOps)**: Zero-Trust Networking (Tailscale), Network Segmentation, Infrastructure Hardening.
* **IoT & Hardware Integration**: Reverse-Engineering BLE Protocols, Custom Firmware Bridges, State Management.
* **AI-Accelerated Development**: Utilizing AI as a pair programmer with a focus on "Context Engineering" to increase development velocity.

## Security

This stack implements defense-in-depth security practices suitable for IoT deployments:

### Why `privileged: true` Was Removed

Running containers with `privileged: true` grants **full host access** equivalent to exposing the Docker daemon socket. This is a critical security risk because:

- Containers can modify kernel parameters, mount filesystems, and access all devices
- A compromised container can break out to the host system
- IoT stacks are high-value targets for attackers seeking to pivot into home networks
- The principle of least privilege requires granting only necessary permissions

### Capabilities Granted and Why

Instead of privileged mode, services receive **surgical capability grants**:

#### Home Assistant
- `NET_ADMIN`: Required for network management (mDNS, HomeKit discovery, network scanning)
- `NET_RAW`: Enables raw socket access for BLE and network device discovery
- `cap_drop: ALL`: Drops all capabilities first, then adds back only what's needed
- Device access: `/dev/ttyUSB0`, `/dev/ttyACM0` for Zigbee/Z-Wave sticks via explicit device mapping
- `network_mode: host`: Required for mDNS/HomeKit/local discovery protocols

#### Bridge (BLE/MQTT)
- `NET_ADMIN`: Network management for Bluetooth operations
- `NET_RAW`: Raw socket access for Bluetooth Low Energy communication
- `cap_drop: ALL`: Drops all capabilities first, then adds back only what's needed
- Device access: `/dev/hci0` for Bluetooth HCI device via explicit device mapping
- `network_mode: host`: Required for Bluetooth stack access

#### Mosquitto (MQTT Broker)
- `cap_drop: ALL`: Drops all capabilities (no special privileges needed)
- `read_only: true`: Filesystem is read-only except for tmpfs
- `tmpfs: /tmp`: Temporary filesystem for runtime data

All services include:
- `security_opt: [no-new-privileges:true]`: Prevents privilege escalation
- Healthchecks: Automated container health monitoring
- Pinned versions: No `:latest` tags for reproducibility

### How to Add New Devices Securely

When adding hardware devices (sensors, radios, serial devices):

1. **Identify the device path** on the host:
   ```bash
   ls -l /dev/tty* /dev/hci*
   ```

2. **Add explicit device mapping** in `docker-compose.yml`:
   ```yaml
   devices:
     - /dev/ttyUSB1:/dev/ttyUSB1  # New Zigbee stick
   ```

3. **NEVER use `privileged: true`** to work around device access issues

4. **Check required capabilities**:
   - Serial/USB devices: Usually no extra capabilities needed
   - Network devices: May need `NET_ADMIN`, `NET_RAW`
   - GPIO/SPI/I2C: Consider group membership or device permissions

### Critical Warning: Docker Daemon Socket

**NEVER** expose the Docker daemon socket (`/var/run/docker.sock`) to containers in this stack. This would grant:
- Full container orchestration control
- Ability to start privileged containers
- Complete host system compromise potential

If container management is needed, use dedicated tools like Portainer with restricted access patterns.

### Additional Security Layers

- **Network Isolation**: Stack uses `network_mode: host` only where required (mDNS, BLE)
- **Volume Permissions**: All config volumes mounted with appropriate read-only flags
- **Restart Policies**: `unless-stopped` prevents runaway restart loops
- **Version Pinning**: All images use specific version tags for supply chain security
- **Secrets Management**: Sensitive data in `secrets.yaml` (excluded from Git)

For external access, deploy behind a reverse proxy (Caddy/Traefik/Nginx) with TLS and authentication.
