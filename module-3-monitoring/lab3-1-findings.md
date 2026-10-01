# Lab 3.1 — Uptime Kuma Monitoring

## Objective

The objective of this lab was to deploy Uptime Kuma and monitor the availability of a web server, SSH service, and VM network reachability.

## Uptime Kuma Setup

Uptime Kuma was deployed using Docker on the monitoring VM.

The monitored target VM IP was:

172.31.13.177

## Monitors Created

### 1. Lab Nginx Server

- Monitor Type: HTTP(s)
- URL: http://172.31.13.177
- Heartbeat Interval: 60 seconds
- Retries: 3
- Purpose: To monitor whether the Nginx web server is available and responding to HTTP requests.

### 2. Lab SSH Port

- Monitor Type: TCP Port
- Host: 172.31.13.177
- Port: 22
- Heartbeat Interval: 60 seconds
- Purpose: To verify that the SSH service is reachable on the target VM.

### 3. Lab VM Reachability

- Monitor Type: Ping
- Host: 172.31.13.177
- Heartbeat Interval: 30 seconds
- Purpose: To verify basic network connectivity and reachability of the target VM.

## Failure Test

Nginx was intentionally stopped using:

sudo systemctl stop nginx

Uptime Kuma detected the failure and changed the **Lab Nginx Server** monitor to **Down** with a connection refused error.

The SSH and VM Reachability monitors remained available.

## Recovery Test

Nginx was restarted using:

sudo systemctl start nginx

After the next monitoring checks, the **Lab Nginx Server** monitor returned to **Up** status.

This demonstrated automatic failure detection and recovery monitoring.

## Why Monitor a Gateway / Server?

Monitoring a gateway or server is important because it helps detect service availability problems quickly.

If a gateway or server becomes unavailable, users may lose access to applications or network resources. Monitoring can detect the failure, provide an alert, and help identify the affected service so that the issue can be investigated and restored.
