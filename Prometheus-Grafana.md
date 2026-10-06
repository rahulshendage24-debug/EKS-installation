# Prometheus + Grafana Installation on Kubernetes using Helm

This guide explains how to install:

- Prometheus
- Grafana
- Alertmanager
- Node Exporter
- Kube State Metrics

using the **kube-prometheus-stack** Helm chart.

---

# Prerequisites

- Kubernetes Cluster Installed
- kubectl Configured
- Internet Access
- Root/Sudo Access

Verify Kubernetes:

```bash
kubectl get nodes
```

Expected Output:

```text
NAME              STATUS   ROLES
master-node       Ready    control-plane
```

---

# Step 1: Install curl

Install curl if not already installed.

For RHEL/CentOS/Amazon Linux:

```bash
yum install -y curl
```

Verify:

```bash
curl --version
```

---

# Step 2: Install Helm

Download Helm installation script:

```bash
curl -fsSL https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 -o get_helm.sh
```

Provide execute permission:

```bash
chmod 700 get_helm.sh
```

Install Helm:

```bash
./get_helm.sh
```

Verify installation:

```bash
helm version
```

Expected Output:

```text
version.BuildInfo{Version:"v3.x.x"}
```

---

# Step 3: Add Prometheus Helm Repository

Add repository:

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
```

Update repositories:

```bash
helm repo update
```

Verify:

```bash
helm repo list
```

---

# Step 4: Create Monitoring Namespace

```bash
kubectl create namespace monitoring
```

Verify:

```bash
kubectl get namespaces
```

Expected Output:

```text
monitoring
```

---

# Step 5: Install Prometheus and Grafana

Install kube-prometheus-stack:

```bash
helm install monitoring prometheus-community/kube-prometheus-stack \
  --namespace monitoring
```

Check Helm Release:

```bash
helm list -n monitoring
```

Expected Output:

```text
NAME         NAMESPACE    STATUS
monitoring   monitoring   deployed
```

---

# Step 6: Verify Installation

Check Pods:

```bash
kubectl get pods -n monitoring
```

Expected Output:

```text
monitoring-grafana
monitoring-kube-prometheus-prometheus
monitoring-kube-state-metrics
monitoring-prometheus-node-exporter
```

All pods should be:

```text
Running
```

---

# Step 7: Check Services

```bash
kubectl get svc -n monitoring
```

Example Output:

```text
NAME                                      TYPE        CLUSTER-IP
monitoring-grafana                        ClusterIP
monitoring-kube-prometheus-prometheus     ClusterIP
```

---

# Step 8: Expose Grafana using NodePort

Convert Grafana Service:

```bash
kubectl patch svc monitoring-grafana \
-n monitoring \
-p '{"spec":{"type":"NodePort"}}'
```

Verify:

```bash
kubectl get svc monitoring-grafana -n monitoring
```

Example Output:

```text
NAME                 TYPE       PORT(S)
monitoring-grafana   NodePort   80:31152/TCP
```

NodePort:

```text
31152
```

---

# Step 9: Get Grafana Admin Password

```bash
kubectl get secret monitoring-grafana \
-n monitoring \
-o jsonpath="{.data.admin-password}" | base64 -d
```

Example Output:

```text
neCvuVb5Cz2Cdd5JrHEWqMot5hk
```

Default Username:

```text
admin
```

---

# Step 10: Access Grafana

Open Browser:

```text
http://<EC2-PUBLIC-IP>:31152
```

Example:

```text
http://13.201.129.84:31152
```

Login:

```text
Username: admin
Password: <password from Step 9>
```

---

# Step 11: Expose Prometheus using NodePort

Convert Prometheus Service:

```bash
kubectl patch svc monitoring-kube-prometheus-prometheus \
-n monitoring \
-p '{"spec":{"type":"NodePort"}}'
```

Verify:

```bash
kubectl get svc monitoring-kube-prometheus-prometheus -n monitoring
```

Example Output:

```text
NAME                                      TYPE       PORT(S)
monitoring-kube-prometheus-prometheus     NodePort   9090:32090/TCP
```

NodePort:

```text
32090
```

---

# Step 12: Access Prometheus

Open Browser:

```text
http://<EC2-PUBLIC-IP>:32090
```

Example:

```text
http://13.201.129.84:32090
```

---

# AWS Security Group Configuration

If Grafana or Prometheus is not accessible:

Go to:

```text
AWS Console
→ EC2
→ Security Groups
→ Inbound Rules
```

Add:

| Type | Protocol | Port |
|--------|----------|--------|
| Custom TCP | TCP | 31152 |
| Custom TCP | TCP | 32090 |

Source:

```text
0.0.0.0/0
```

Or use your organization's IP range.

---

# Verification Commands

Check Pods:

```bash
kubectl get pods -n monitoring
```

Check Services:

```bash
kubectl get svc -n monitoring
```

Check Helm Release:

```bash
helm list -n monitoring
```

Check Node Status:

```bash
kubectl get nodes
```

---

# Useful Commands

View Grafana Logs:

```bash
kubectl logs -n monitoring deployment/monitoring-grafana
```

View Prometheus Pods:

```bash
kubectl get pods -n monitoring | grep prometheus
```

Describe Grafana Service:

```bash
kubectl describe svc monitoring-grafana -n monitoring
```

Describe Prometheus Service:

```bash
kubectl describe svc monitoring-kube-prometheus-prometheus -n monitoring
```

---

# Uninstall Prometheus and Grafana

Remove Helm Release:

```bash
helm uninstall monitoring -n monitoring
```

Delete Namespace:

```bash
kubectl delete namespace monitoring
```

Verify Removal:

```bash
kubectl get namespaces
```

---

# Architecture

```text
+--------------------+
|      Browser       |
+----------+---------+
           |
           |
           v
+--------------------+
|     NodePort       |
| Grafana : 31152    |
| Prometheus : 32090 |
+----------+---------+
           |
           |
           v
+-------------------------------+
| Kubernetes Monitoring Stack   |
+-------------------------------+
| Grafana                       |
| Prometheus                    |
| Alertmanager                  |
| Node Exporter                 |
| Kube State Metrics            |
+-------------------------------+
```

---

# Author

DevOps Monitoring Setup Guide

Prometheus + Grafana on Kubernetes using Helm
