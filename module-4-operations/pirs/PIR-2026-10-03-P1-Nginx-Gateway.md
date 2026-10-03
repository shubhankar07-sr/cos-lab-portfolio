# PIR — P1 Nginx Gateway Incident

## 1. Incident Summary

A P1 incident was simulated by stopping the Nginx service on the gateway VM.

Uptime Kuma detected the gateway as DOWN and reported a connection refusal on port 80.

- Incident: Lab Nginx Gateway Unreachable
- Priority: P1-Critical
- Affected Service: Nginx Gateway
- Detection Tool: Uptime Kuma
- Investigation Tool: Prometheus
- Status: Resolved

## 2. Impact

The Nginx gateway became unavailable and HTTP requests to port 80 could not be served.

Uptime Kuma reported:

`connect ECONNREFUSED 172.31.13.177:80`

This simulated a gateway outage where users would be unable to access the service through the gateway.

## 3. Detection and Response Timeline

| Time | Action |
|---|---|
| T+0 | Uptime Kuma detected the Nginx gateway as DOWN |
| T+2 | P1 GitHub Issue created |
| T+3 | Customer acknowledgement recorded |
| T+5 | Uptime Kuma incident timeline investigated |
| T+8 | Prometheus CPU metrics checked |
| T+10 | Internal investigation findings documented |
| T+20 | Nginx service restarted |
| T+22 | Uptime Kuma confirmed recovery with HTTP 200 OK |
| T+25 | Resolution notice documented |
| T+30 | Incident closed |

## 4. Root Cause

The Nginx service running on the gateway VM was stopped during the P1 simulation.

Because Nginx was not running, port 80 refused incoming HTTP connections. Uptime Kuma therefore detected the gateway as unavailable.

Prometheus CPU monitoring showed no significant CPU spike during the incident, so CPU resource exhaustion was not identified as the cause.

## 5. Resolution

The Nginx service was restarted using:

```bash
sudo systemctl start nginx
