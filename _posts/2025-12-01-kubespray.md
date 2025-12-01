docker run --rm -it --mount type=bind,source="$(pwd)"/inventory/sample,dst=/inventory \
  --mount type=bind,source="${HOME}"/.ssh/id_ed25519,dst=/root/.ssh/id_ed25519 \
  quay.io/kubespray/kubespray:v2.29.0 bash



## how to enable kubevip loadbalancer, addons.yml

# Enable kube-vip
kube_vip_enabled: true

# Virtual IP for your API server (make sure it's free in your network)
kube_vip_address: 192.168.100.10

# Load balancer configuration
loadbalancer_apiserver:
  address: "{{ kube_vip_address }}"
  port: 6443

# Network interface to bind the VIP
kube_vip_interface: eth0

# ARP mode (Layer2) for local network
kube_vip_arp_enabled: true
kube_vip_controlplane_enabled: true

# Additional options
kube_vip_services_enabled: false
kube_vip_dns_mode: first
kube_vip_cp_detect: false
kube_vip_lb_fwdmethod: local
kube_vip_enable_node_labeling: false

# Force pods to run in hostNetwork with NET_ADMIN to guarantee VIP assignment
kube_vip_pod_hostnetwork: true
kube_vip_pod_capabilities:
  add:
    - NET_ADMIN

## force arp mode, k8s-cluster.yml

kube_proxy_strict_arp: true
