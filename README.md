# Build-K3S-with-HA-with-embedded-DB
Build K3s with High Availability with Embedded DB


1. First master node:
```
sudo mkdir /etc/rancher
sudo mkdir /etc/rancher/k3s
sudo vi  /etc/rancher/k3s/config.yaml

kubelet-arg: "node-status-update-frequency=4s" 
kube-controller-manager-arg: "node-monitor-period=4s" 
kube-controller-manager-arg: "node-monitor-grace-period=16s" 
kube-controller-manager-arg: "pod-eviction-timeout=20s" 
kube-apiserver-arg: "default-not-ready-toleration-seconds=20" 
kube-apiserver-arg: "default-unreachable-toleration-seconds=20" 
write-kubeconfig-mode: 644

sudo chmod 644  /etc/rancher/k3s/config.yaml

curl -sfL https://get.k3s.io | INSTALL_K3S_EXEC="server --cluster-init" K3S_CONFIG_FILE="/etc/rancher/k3s/config.yaml" sh -
```


2. Subsequent master node:

```
curl -sfL https://get.k3s.io | K3S_TOKEN=<token of first node> INSTALL_K3S_EXEC="server --server https://<IP address of first node>:6443" sh -
```



3. Worker nodes joining cluster:

```
curl -sfL https://get.k3s.io | K3S_URL=https://<master IP address>:6443 K3S_TOKEN=<Token> sh -
```
