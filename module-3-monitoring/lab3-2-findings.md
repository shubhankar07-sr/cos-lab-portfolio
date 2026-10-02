# Lab 3.2 – Monitoring with Prometheus and Grafana

## Objective

The objective of this lab was to build a monitoring system using Prometheus,
Node Exporter and Grafana for monitoring VM performance metrics.

## Monitoring Stack

- Prometheus – collects and stores time-series monitoring metrics.
- Node Exporter – exposes Linux system metrics to Prometheus.
- Grafana – visualizes the metrics through dashboards and supports alerting.
- Node Exporter Full – imported using Grafana dashboard ID 1860.

## Grafana Dashboard

The Grafana dashboard contains monitoring panels for:

1. CPU Usage (%)
2. Memory Usage (%)
3. Network Traffic

The Node Exporter Full dashboard was also imported to provide detailed
VM-level monitoring.

## CPU Alert Configuration

A CPU alert was configured with the following condition:

- Condition: CPU usage is above 80%
- Pending period: 1 minute
- Contact point: Lab 3.2 CPU Alert

The alert was tested by generating CPU load on the monitoring VM.

The CPU usage crossed the 80% threshold and the alert entered the
Firing state.

After stopping the CPU-load processes, the CPU usage returned to normal
and the alert recovered to the Normal state.

## Prometheus/Grafana vs SigNoz

### Prometheus and Grafana

Prometheus is used to collect and query time-series metrics such as CPU,
memory, disk and network usage. Grafana provides dashboards and
visualizations for these metrics and can also be used for alerting.

### SigNoz

SigNoz is an observability platform that can be used to investigate
application and system observability data.

### During a P1 Incident

Prometheus/Grafana can be used when the incident requires investigation
of infrastructure metrics such as CPU, memory, disk or network usage.

SigNoz can be used when the incident requires investigation of
application observability and related telemetry.

Both tools can support incident investigation from different monitoring
and observability perspectives.

## Conclusion

The lab successfully demonstrated VM monitoring using Prometheus,
Node Exporter and Grafana. A multi-panel Grafana dashboard was created,
a CPU alert threshold was configured and tested, and the Node Exporter
Full dashboard was imported using dashboard ID 1860.
