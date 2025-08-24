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
