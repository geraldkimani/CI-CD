# Prometheus + Grafana Monitoring on Kubernetes (EKS) - Commands Reference

## 1. Create Monitoring Namespace

```bash
kubectl create namespace monitoring
```

---

# 2. Add Helm Repository

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
```

---

# 3. Update Helm Repositories

```bash
helm repo update
```

---

# 4. Install kube-prometheus-stack

```bash
helm install monitoring prometheus-community/kube-prometheus-stack \
  --namespace monitoring
```

---

# 5. Verify Monitoring Pods

```bash
kubectl get pods -n monitoring
```

---

# 6. Verify Monitoring Services

```bash
kubectl get svc -n monitoring
```

---

# 7. Access Grafana

```bash
kubectl port-forward svc/monitoring-grafana 3000:80 -n monitoring
```

Open:

```text
http://localhost:3000
```

---

# 8. Get Grafana Admin Password

```bash
kubectl get secret monitoring-grafana -n monitoring \
  -o jsonpath="{.data.admin-password}" | base64 --decode
```

Grafana Username:

```text
admin
```

---

# 9. Access Prometheus

```bash
kubectl port-forward svc/monitoring-kube-prometheus-prometheus 9090:9090 -n monitoring
```

Open:

```text
http://localhost:9090
```

---

# 10. Useful PromQL Queries

## Cluster CPU Usage

```promql
100 - (avg by(instance)(rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100)
```

---

## Memory Usage

```promql
100 * (1 - (node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes))
```

---

## Pod Status

```promql
kube_pod_status_phase
```

---

## Network Receive Traffic

```promql
rate(node_network_receive_bytes_total[5m])
```

---

## Network Transmit Traffic

```promql
rate(node_network_transmit_bytes_total[5m])
```

---

## Storage Usage

```promql
100 * (1 - (node_filesystem_avail_bytes / node_filesystem_size_bytes))
```

---

## Average CPU Over Time

```promql
avg_over_time(node_load1[10m])
```

---

# 11. Prometheus Data Source URL

```text
http://monitoring-kube-prometheus-prometheus.monitoring:9090
```

---

# 12. Apply Alert Rules

```bash
kubectl apply -f alert-rules.yaml
```

---

# 13. Create CPU Load Generator

```bash
kubectl run cpu-loader \
  --image=busybox \
  --namespace default \
  -- /bin/sh -c "while true; do :; done"
```

---

# 14. Access Alertmanager

```bash
kubectl port-forward svc/monitoring-kube-prometheus-alertmanager 9093:9093 -n monitoring
```

Open:

```text
http://localhost:9093
```

---

# 15. Delete CPU Load Generator

```bash
kubectl delete pod cpu-loader
```

---

# 16. Monitoring and Troubleshooting Commands

## Get Monitoring Pods

```bash
kubectl get pods -n monitoring
```

---

## View Grafana Logs

```bash
kubectl logs -n monitoring deployment/monitoring-grafana
```

---

## Describe Pod

```bash
kubectl describe pod -n monitoring <pod-name>
```

---

## View Events

```bash
kubectl get events -n monitoring
```

---

## View Node Resource Usage

```bash
kubectl top nodes
```

---

## View Pod Resource Usage

```bash
kubectl top pods -A
```

---

# 17. Helm Management Commands

## List Helm Releases

```bash
helm list -n monitoring
```

---

## Upgrade Helm Release

```bash
helm upgrade monitoring prometheus-community/kube-prometheus-stack \
  --namespace monitoring
```

---

## Uninstall Helm Release

```bash
helm uninstall monitoring -n monitoring
```

---

# 18. Cleanup Commands

## Delete Monitoring Namespace

```bash
kubectl delete namespace monitoring
```

---

# End of Prometheus + Grafana Commands Reference
