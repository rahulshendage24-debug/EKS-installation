 ==========================================
 Prometheus + Grafana Setup (Kubernetes)
 ==========================================

============================
install Helm directly
===========================

1. curl -fsSL https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 -o get_helm.sh

2. chmod 700 get_helm.sh

3. ./get_helm.sh
 
4. helm --version
===============================================================================
INSTALL THE CURL COMMAND
======================================================
yum install -y curl

======================================================================
Add the Prometheus Helm repository
=====================================================================

1. helm repo add prometheus-community https://prometheus-community.github.io/helm-charts

2. helm repo update

3.  Create a monitoring namespace

4.  kubectl create namespace monitoring

5. Install Prometheus and Grafana

6. helm install monitoring prometheus-community/kube-prometheus-stack \
  --namespace monitoring

7. Check installation

    kubectl get pods -n monitoring
    kubectl get svc -n monitoring
8. Get Grafana password

kubectl get secret monitoring-grafana \
  -n monitoring \
  -o jsonpath="{.data.admin-password}" | base64 -d

9. Expose Grafana using NodePort

 kubectl patch svc monitoring-grafana \
  -n monitoring \
  -p '{"spec":{"type":"NodePort"}}'

  kubectl get svc monitoring-grafana -n monitoring


10. Access Prometheus

kubectl patch svc monitoring-kube-prometheus-prometheus \
  -n monitoring \
  -p '{"spec":{"type":"NodePort"}}'

kubectl get svc monitoring-kube-prometheus-prometheus -n monitoring

