# Lab 5.1 — Kill the Gateway: Simulate and Respond to a P1

## 1. Incident Summary

A P1 incident was simulated by stopping the Nginx service on the gateway VM. Uptime Kuma detected the gateway as DOWN because port 80 stopped accepting connections.

- **Incident:** Lab Nginx Gateway Unreachable
- **Priority:** P1-Critical
- **Affected Service:** Nginx Gateway
- **Affected IP:** 172.31.13.177
- **Detection Tool:** Uptime Kuma
- **Monitoring Tool:** Prometheus
- **Root Cause:** Nginx service was stopped
- **Resolution:** Nginx service was restarted successfully

## 2. Detection

Uptime Kuma detected the Nginx gateway as DOWN and reported:

`connect ECONNREFUSED 172.31.13.177:80`

The incident was then recorded as a GitHub Issue:

`[P1] Lab Nginx Gateway Unreachable – All Users Offline`

Labels used:

- `P1-Critical`
- `product:ztna`
- `status:in-progress`

## 3. Investigation

Uptime Kuma showed the gateway failure on the HTTP monitor.

Prometheus CPU monitoring was checked to determine whether the incident was caused by high CPU usage. CPU usage remained low and there was no significant CPU spike around the investigation period.

The evidence indicated that the problem was with the Nginx service rather than CPU resource exhaustion.

## 4. Root Cause

The Nginx service on the gateway VM was stopped during the P1 simulation.

As a result, port 80 stopped accepting HTTP connections and Uptime Kuma reported the gateway as DOWN.

## 5. Resolution

The Nginx service was restarted using:

```bash
sudo systemctl start nginx
