Resilient & Secure Smart Home Infrastructure Stack
This repository contains the core Docker Compose stack for a multi-layered, secure, and resilient smart home lab. This project demonstrates a practical application of DevOps principles, IoT integration, and network security to create a stable and extensible home automation ecosystem.

The architecture is designed for high availability and systematic troubleshooting, bridging proprietary IoT hardware with open-source platforms through custom middleware.

Architecture Overview

This stack is the heart of a larger DevSecOps environment, running containerized services that are securely accessible via a Tailscale mesh VPN. It integrates custom-built components to control hardware that lacks official support, showcasing a full-cycle engineering process from reverse engineering to containerized deployment.

Core Components & Skills Demonstrated

1. ha-iot-stack (This Repository)

The central orchestration layer for the smart home.

Technology: Docker, Docker Compose, Home Assistant, Mosquitto (MQTT).

Skills:

Containerization & Orchestration: Architected a multi-container application, managing dependencies, networking, and persistent data volumes.

Infrastructure as Code (IaC): Defined the entire smart home core declaratively, enabling reproducible deployments.

Systematic Debugging: Performed root cause analysis on complex, cascading failures across container, OS, and hardware layers.

2. ha-ble-mqtt-bridge (Custom Middleware)

A custom Python application built to reverse-engineer and control a proprietary Bluetooth LE LED controller, bridging it to the MQTT network for Home Assistant integration.

Technology: Python, bleak library, paho-mqtt, Bluetooth Low Energy (BLE).

Skills:

IoT Reverse Engineering: Utilized packet sniffing tools (gatttool) to decode an undocumented BLE protocol and replicate its command structure.

Resilience Engineering: Implemented a "software power cycle" and an aggressive reconnection strategy in Python to handle the device's unstable firmware, ensuring reliable state changes.

Software Development: Authored a robust, containerized Python service to act as a hardware-to-protocol bridge, a common pattern in IoT.

3. rpispy-tailscale-autoheal (Network Resilience)

A self-healing system deployed on a Raspberry Pi to ensure 24/7 network uptime for the lab's secure Tailscale mesh VPN, acting as a subnet router and exit node.

Technology: Bash, cron, Tailscale API, Linux System Administration.

Skills:

Network Automation & Security: Deployed a zero-trust mesh network to secure lab access, eliminating the need for open ports.

Proactive Monitoring & Healing: Created an automated script to detect and remediate VPN connection issues, ensuring persistent and reliable remote access to critical infrastructure.

Global Skills Showcase

DevOps & Automation: CI/CD pipeline authoring (GitHub Actions), containerization (Docker), infrastructure orchestration.

Security (SecOps): Zero-trust networking (Tailscale), network segmentation (VLANs), developing threat detection systems (NullPointVector), infrastructure hardening.

IoT & Hardware Integration: Reverse-engineering BLE protocols, writing custom firmware bridges, state management for unreliable hardware.

AI-Accelerated Development: Utilized AI as a pair programmer and knowledge base, focusing on "Context Engineering" to provide precise prompts and validate outputs, drastically increasing development velocity and troubleshooting efficiency.

Future Development

This infrastructure serves as the foundation for more advanced projects currently in development:

24/7 On-Premise Server: A dedicated server for centralized storage, surveillance footage processing, and expanded compute capabilities.

NullPointVector: A full-scale, multi-channel Intrusion Detection & Prevention System (IDPS) for phishing, smishing, and vishing threats.
