# Prometheus + Grafana Setup (Kubernetes)

---

## Install Helm Directly

```bash
curl -fsSL https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 -o get_helm.sh
chmod 700 get_helm.sh
./get_helm.sh
helm --version
```

---

## Install the Curl Command

```bash
yum install -y curl
```

---

## Add the Prometheus Helm Repository

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

---

## Create a Monitoring Namespace

```bash
kubectl create namespace monitoring
```

---

## Install Prometheus and Grafana

```bash
helm install monitoring prometheus-community/kube-prometheus-stack \
  --namespace monitoring
```

---

## Check Installation

```bash
kubectl get pods -n monitoring
kubectl get svc -n monitoring
```

---

## Get Grafana Password

```bash
kubectl get secret monitoring-grafana \
  -n monitoring \
  -o jsonpath="{.data.admin-password}" | base64 -d
```

---

## Expose Grafana using NodePort

```bash
kubectl patch svc monitoring-grafana \
  -n monitoring \
  -p '{"spec":{"type":"NodePort"}}'

kubectl get svc monitoring-grafana -n monitoring
```

---

## Access Prometheus

```bash
kubectl patch svc monitoring-kube-prometheus-prometheus \
  -n monitoring \
  -p '{"spec":{"type":"NodePort"}}'

kubectl get svc monitoring-kube-prometheus-prometheus -n monitoring
```
