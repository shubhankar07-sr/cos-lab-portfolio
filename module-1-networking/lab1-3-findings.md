# Lab 1.3 — WireGuard VPN / Encrypted Tunnel Findings

## Objective

Configure and verify a WireGuard VPN tunnel between two Ubuntu 22.04 AWS EC2 instances.

- WireGuard Server tunnel IP: `10.0.0.1/24`
- WireGuard Client tunnel IP: `10.0.0.2/24`
- WireGuard UDP port: `51820`
- Server AWS interface: `ens5`

## 1. WireGuard Handshake Verification

The WireGuard tunnel was successfully established between the client and server.

Server `wg show` confirmed:

- Interface: `wg0`
- Listening port: `51820`
- Client peer allowed IP: `10.0.0.2/32`
- Latest handshake: successful
- Data transfer: received and sent

Client `wg show` also confirmed:

- Peer endpoint: `52.66.109.44:51820`
- Allowed IPs: `10.0.0.0/24`
- Latest handshake: successful
- Persistent keepalive: `25 seconds`

## 2. VPN Tunnel Connectivity Test

From the WireGuard client:

```text
ping -c 4 10.0.0.1
