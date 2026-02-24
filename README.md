# Kubernetes Observability Lab

## Overview

This project deploys a full observability stack on Minikube using Helm.

Components deployed:

- Prometheus (metrics collection)
- Grafana (dashboards & visualization)
- Loki (centralized logging)
- Fluent Bit (log collection)
- Alertmanager (alert handling)

Two isolated environments were created:

- monitoring-dev
- monitoring-prod

Each environment is configured using separate Helm values files to ensure Dev/Prod parity.

---

# Repository Structure

.
├── values-dev.yaml
├── values-prod.yaml
├── loki-values-dev.yaml
├── loki-values-prod.yaml
├── k8s/namespaces.yaml
└── README.md

---

# Deployment Approach

All components were deployed using:

helm upgrade --install <release> <chart> -n <namespace> -f <values-file>

## Why use `helm upgrade --install`?

- Avoids errors if the release already exists.
- Single command for install and upgrade.
- Ensures idempotency.
- Supports declarative / GitOps-style workflows.

---

# Dev / Prod Parity

Both environments use the same Helm chart but different values files:

- values-dev.yaml
- values-prod.yaml

## Key Differences

| Setting | Dev | Prod |
|----------|------|------|
| Prometheus retention | 7d | 30d |
| CPU requests | 200m | 500m |
| CPU limits | 500m | 1 |
| Memory requests | 512Mi | 1Gi |
| Memory limits | 1Gi | 2Gi |
| Grafana admin password | devadmin | prodadmin |

Both were deployed into separate namespaces:

- monitoring-dev
- monitoring-prod

This ensures structural parity with environment-specific configuration.

---

# Metrics

Prometheus collects Kubernetes metrics such as:

- CPU usage
- Memory usage
- Node metrics
- Pod metrics

Grafana provides:

- Prebuilt Kubernetes dashboards
- Custom dashboard with CPU & Memory panels

---

# Logging

Logging stack includes:

- Fluent Bit (collects container logs)
- Loki (stores and indexes logs)

Logs are queryable in Grafana using LogQL:

{namespace="monitoring-dev"}

Logs are isolated per namespace (dev / prod).

---

# Alerts

A custom Prometheus alert rule was configured:

alert: HighCPU
expr: sum(rate(container_cpu_usage_seconds_total[5m])) > 0.8
for: 2m

## How Alerts Work

1. Prometheus evaluates alert rules.
2. When the condition is true for 2 minutes, the alert fires.
3. Alert is sent to Alertmanager.
4. Alert can be viewed in the Alertmanager UI.

Alert testing was performed using CPU stress on a pod.

---

# Screenshots (Included)

- Grafana dashboards
- Prometheus targets
- Loki logs
- Alertmanager alerts

---

# Conclusion

This lab demonstrates:

- Declarative Helm-based deployments
- Namespace isolation
- Dev/Prod configuration differences
- Full observability stack (metrics, logs, alerts)
- Idempotent infrastructure management

The deployment is reproducible and GitOps-friendly.

---

# Screenshots

The following screenshots are included in the repository under the `screenshots/` directory:

## Grafana Dashboards

- Cluster Dashboard  
  ![Cluster Dashboard](screenshots/grafana-cluster-dash.png)

- Node Dashboard  
  ![Node Dashboard](screenshots/grafana-node-dash.png)

- General Dashboards View  
  ![Dashboards](screenshots/grafana-dashboards.png)

## Loki Logs

- Log exploration in Grafana  
  ![Loki Logs](screenshots/grafana-loki-logs.png)

## Alertmanager

- Alert visibility in Alertmanager UI  
  ![Alertmanager](screenshots/alertmenager.png)

