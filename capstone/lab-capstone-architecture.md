# Capstone — Mini ZTNA Architecture

## 1. Overview

This capstone demonstrates a small ZTNA-like architecture using the cloud lab components built during the previous modules.

The architecture connects a Windows laptop to a WireGuard tunnel hosted on VM2. Nginx on VM2 acts as the gateway/reverse proxy and forwards `/app` requests to Keycloak running on VM1.

## 2. Architecture

```text
                    Windows Laptop
                         |
                         | WireGuard
                         | 10.0.0.2
                         v
                +----------------------+
                | VM2 - Gateway        |
                | 10.0.0.1             |
                | WireGuard + Nginx    |
                +----------+-----------+
                           |
                           | Nginx reverse proxy
                           | /app
                           v
                +----------------------+
                | VM1 - Identity/App   |
                | 172.31.13.177        |
                | Keycloak :8080       |
                +----------------------+

Monitoring:
Uptime Kuma + Prometheus monitor the lab infrastructure.
