# Kubernetes Installation Guide

## 1. Prerequisites (Run on BOTH Master and Worker Nodes)

### Install and Configure Docker
```bash
yum install docker -y
systemctl start docker
systemctl enable docker
systemctl status docker
```

### Configure Kubernetes Repository
This step overwrites any existing configuration in `/etc/yum.repos.d/kubernetes.repo`.

```bash
cat <<EOF | sudo tee /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://pkgs.k8s.io/core:/stable:/v1.35/rpm/
enabled=1
gpgcheck=1
gpgkey=https://pkgs.k8s.io/core:/stable:/v1.35/rpm/repodata/repomd.xml.key
exclude=kubelet kubeadm kubectl cri-tools kubernetes-cni
EOF
```

### Install Kubernetes Components
```bash
sudo yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes
sudo systemctl enable --now kubelet
```

---

## 2. Cluster Initialization (Run on MASTER / Control Plane Node ONLY)

1. Initialize the cluster:
   ```bash
   kubeadm init
   ```
2. Configure local `kubectl` access:
   ```bash
   mkdir -p $HOME/.kube
   sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
   sudo chown $(id -u):$(id -g) $HOME/.kube/config
   ```
3. Export the environment variable:
   ```bash
   export KUBECONFIG=/etc/kubernetes/admin.conf
   ```
4. **Join Worker Nodes:** Copy the `kubeadm join` token printed output from this master node initialization, and execute it on your worker nodes to connect them to the cluster.
5. Verify initial node status:
   ```bash
   kubectl get nodes
   ```

---

## 3. Network Configuration (Run on MASTER / Control Plane Node ONLY)

For comprehensive networking configuration details, reference the official [Tigera Calico Documentation](https://docs.tigera.io/calico/latest/getting-started/kubernetes/self-managed-onprem/onpremises).

1. Download the Calico networking manifest for the Kubernetes API datastore:
   ```bash
   curl https://raw.githubusercontent.com/projectcalico/calico/v3.32.2/manifests/calico.yaml -O
   ```
2. Apply the manifest to your cluster:
   ```bash
   kubectl apply -f calico.yaml
   ```

---

## 4. Verification

Once the networking configuration is applied, run the following commands to confirm your Kubernetes configuration is running successfully:

1. Check your cluster nodes status:
   ```bash
   kubectl get nodes
   ```
2. Check core system components health status:
   ```bash
   kubectl get pods -n kube-system
