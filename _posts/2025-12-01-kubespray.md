---

# 🚀 Enabling **kube-vip Load Balancer** in Kubespray  
### *Using Kubespray v2.29.0 inside Docker*

---

## 🐳 **Run Kubespray container**

```bash
docker run --rm -it \
  --mount type=bind,source="$(pwd)"/inventory/sample,dst=/inventory \
  --mount type=bind,source="${HOME}"/.ssh/id_ed25519,dst=/root/.ssh/id_ed25519 \
  quay.io/kubespray/kubespray:v2.29.0 bash
```

---

# ⚙️ kube-vip Configuration

You will edit:

- `inventory/sample/group_vars/k8s-cluster/addons.yml`  
- `inventory/sample/group_vars/k8s-cluster/k8s-cluster.yml`  

---

## 🟦 **addons.yml — Enable kube-vip**

```yaml
# Enable kube-vip for API server load balancing
kube_vip_enabled: true

# Virtual IP for your Kubernetes control-plane
kube_vip_address: 192.168.100.10

# API Server Load Balancer
loadbalancer_apiserver:
  address: "{{ kube_vip_address }}"
  port: 6443

# Interface where the VIP will be bound
kube_vip_interface: eth0

# Enable ARP mode (Layer2)
kube_vip_arp_enabled: true
kube_vip_controlplane_enabled: true

# Service LoadBalancer functionality (disabled)
kube_vip_services_enabled: false

# DNS resolution mode
kube_vip_dns_mode: first

# Misc kube-vip options
kube_vip_cp_detect: false
kube_vip_lb_fwdmethod: local
kube_vip_enable_node_labeling: false

# Run kube-vip pod in hostNetwork with NET_ADMIN capability
kube_vip_pod_hostnetwork: true
kube_vip_pod_capabilities:
  add:
    - NET_ADMIN
```

---

# 🔧 **k8s-cluster.yml — Required ARP Fix**

kube-vip **requires** strict ARP mode in kube-proxy.

```yaml
kube_proxy_strict_arp: true
```

---

# ✔️ Summary  
This setup:

- Enables kube-vip as the **control-plane load balancer**  
- Uses **Layer2 (ARP) mode**  
- Assigns a static VIP: `192.168.100.10`  
- Ensures kube-proxy is correctly configured (`strict_arp: true`)  
- Runs kube-vip with the necessary NET_ADMIN capability  

Everything is now ready for:

```bash
ansible-playbook -i /inventory/inventory.ini --private-key /root/.ssh/id_ed25519 cluster.yml
```

---
