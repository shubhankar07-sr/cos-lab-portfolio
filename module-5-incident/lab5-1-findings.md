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

## 6. Incident Timeline

| Event | Time (IST) |
|---|---|
| Uptime Kuma detected gateway DOWN | 10:58:09 |
| P1 GitHub ticket created | [GitHub Issue #4 timestamp] |
| Customer acknowledgement posted | [GitHub comment timestamp] |
| Investigation update posted | [GitHub comment timestamp] |
| Nginx service restarted | 11:27:53 |
| Uptime Kuma confirmed recovery | 11:28:10 |

**Total Incident Duration:** 30 minutes 1 second  
(10:58:09 to 11:28:10 IST)

## 7. Resolution Verification

After restarting Nginx, Uptime Kuma confirmed that the gateway recovered successfully.

The monitor returned:

`200 OK`

The Nginx service was confirmed to be active and the gateway became reachable again.

The GitHub P1 issue was updated with the resolution notice and then closed.

## 8. Customer Communication

The P1 acknowledgement was posted to the GitHub ticket shortly after detection.

A T+15 investigation update was also provided to communicate that the investigation was ongoing and that Nginx service status would be verified.

After recovery, the resolution notice was posted confirming that the Nginx service had been restarted and the gateway was operational.

## 9. Lessons Learned

- Uptime Kuma provided rapid detection of the gateway outage.
- Prometheus helped confirm that CPU usage was not the cause of the incident.
- The incident demonstrated the importance of timely P1 acknowledgement and communication.
- Service-level monitoring helped verify successful recovery after restarting Nginx.

## 10. Evidence

- `screenshots/lab5-1-uptime-kuma-alert.png`
- `screenshots/lab5-1-prometheus-cpu-check.png`
- `screenshots/lab5-1-uptime-kuma-recovery.png`
- `screenshots/lab5-1-uptime-red.png`

**P1 Ticket:** GitHub Issue #4 — `[P1] Lab Nginx Gateway Unreachable – All Users Offline`

**PACE Handover:** `module-4-operations/handovers/PACE-2026-10-03-P1-Nginx-Gateway.md`

**PIR:** `module-4-operations/pirs/PIR-2026-10-03-P1-Nginx-Gateway.md`
