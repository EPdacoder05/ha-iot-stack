# Resilient & Secure Smart Home Infrastructure Stack

This document outlines the technical architecture and skills demonstrated in the creation of a multi-layered, secure, and resilient smart home lab. This project serves as a practical application of DevOps principles, IoT integration, and network security to create a stable and extensible home automation ecosystem.

The architecture is designed for high availability and systematic troubleshooting, bridging proprietary IoT hardware with open-source platforms through custom middleware.

### Architecture Overview

The lab is designed as a personal cloud with defined security boundaries and specialized compute zones, all securely accessible via a zero-trust mesh VPN.

```mermaid
graph TD

    subgraph "Core Lab Infrastructure (Workhorse PC / Future Server)"
        F[Docker Engine]
        G(Home Assistant Container)
        H(Mosquitto MQTT Broker)
        I(ha-ble-mqtt-bridge Container)
    end
    
    subgraph "IoT/Edge Devices (IoT VLAN)"
        J(CozyLife BLE Lights)
        K(Security Cameras / Dashcams)
