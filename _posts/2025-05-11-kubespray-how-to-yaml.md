**To install kubespray you need:**
```
docker run --rm -it --mount type=bind,source="$(pwd)"/inventory/sample,dst=/inventory \
  --mount type=bind,source="${HOME}"/.ssh/id_ed25519,dst=/root/.ssh/id_25519 \
  quay.io/kubespray/kubespray:v2.27.0 bash
```

```
all:
  hosts:
    node1:
      ansible_host: 192.168.100.68
      ip: 192.168.100.68
    node2:
      ansible_host: 192.168.100.72
      ip: 192.168.100.72
    node3:
      ansible_host: 192.168.100.74
      ip: 192.168.100.74
  children:
    kube_control_plane:
      hosts:
        node1:
    kube_node:
      hosts:
        node2:
        node3:
    etcd:
      hosts:
        node1:
        node2:
        node3:
    k8s_cluster:
      children:
        kube_control_plane:
        kube_node:
```