### 1. Install HELM

```bash
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```

### 2. Install Kube Prometheus Stack

```bash
# Add Helm repositories
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add stable https://charts.helm.sh/stable
helm repo update

# Create namespace for monitoring
kubectl create namespace monitoring

# Install Kube Prometheus Stack with NodePort services
helm install kind-prometheus prometheus-community/kube-prometheus-stack \
  --namespace monitoring \
  --set prometheus.service.nodePort=30000 \
  --set prometheus.service.type=NodePort \
  --set grafana.service.nodePort=31000 \
  --set grafana.service.type=NodePort \
  --set alertmanager.service.nodePort=32000 \
  --set alertmanager.service.type=NodePort \
  --set prometheus-node-exporter.service.nodePort=32001 \
  --set prometheus-node-exporter.service.type=NodePort

# Verify installation
kubectl get svc -n monitoring
kubectl get namespace

# Expose services via Port Forwarding
kubectl port-forward svc/kind-prometheus-kube-prome-prometheus -n monitoring 9090:9090 --address=0.0.0.0 &
kubectl port-forward svc/kind-prometheus-grafana -n monitoring 31000:80 --address=0.0.0.0 &
```

### 3. Prometheus Queries

**CPU Usage Percentage (Default Namespace)**
```promql
sum (rate (container_cpu_usage_seconds_total{namespace="default"}[1m])) / sum (machine_cpu_cores) * 100
```

**Memory Usage by Pod (Default Namespace)**
```promql
sum (container_memory_usage_bytes{namespace="default"}) by (pod)
```

**Network Receive Bytes by Pod (Default Namespace)**
```promql
sum(rate(container_network_receive_bytes_total{namespace="default"}[5m])) by (pod)
```

**Network Transmit Bytes by Pod (Default Namespace)**
```promql
sum(rate(container_network_transmit_bytes_total{namespace="default"}[5m])) by (pod)
```
