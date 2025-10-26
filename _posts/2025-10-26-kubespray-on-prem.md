## guide to installing kube on prem - pure technical version
https://community-scripts.github.io/ProxmoxVE/scripts?id=ubuntu2404-vm&category=Operating+Systems

create 3 hosts + 1 bastion
ubuntu1
ubuntu2
ubuntu3
ubuntu-bastion

after installation, checkout: ´https://github.com/community-scripts/ProxmoxVE/discussions/272´

```
apt install python3-pip
python3 -m venv kube
source kube/bin/activate
git clone https://github.com/kubernetes-sigs/kubespray.git kubespray
pip install "ansible-core>=2.17.3,<2.18.0"
ansible-playbook -i inventory/mycluster/inventory.ini cluster.yml -b
```


## Guide to installing k8s doc with AI generated elaborate summary

Here’s your guide rewritten as a **complete, well-structured, and informative step-by-step tutorial** for deploying an **on-premises Kubernetes cluster using Kubespray on Proxmox** VMs running **Ubuntu 24.04 LTS**.

---

# 🚀 On-Prem Kubernetes Cluster Setup on Proxmox (Ubuntu 24.04 + Kubespray)

This guide walks you through creating a **3-node Kubernetes cluster** plus **1 bastion host** using **Proxmox VE** and **Kubespray**.  
It assumes you already have **Proxmox VE** installed and accessible.

---

## 🧱 Architecture Overview

| Role        | Hostname | Description |
|--------------|-----------|-------------|
| **Bastion**  | `bastion` | Control machine for Ansible and Kubespray |
| **Control Plane** | `ubuntu1` | Kubernetes master node |
| **Worker Node 1** | `ubuntu2` | Kubernetes worker node |
| **Worker Node 2** | `ubuntu3` | Kubernetes worker node |

---

## 🖥️ Step 1 — Create Virtual Machines on Proxmox

Use the **Proxmox Community Scripts** to automate VM creation.

### 👉 Script Source:
[Proxmox Community Scripts - Ubuntu 24.04 VM](https://community-scripts.github.io/ProxmoxVE/scripts?id=ubuntu2404-vm&category=Operating+Systems)

Each VM should have:
- **2+ CPUs**
- **4–8 GB RAM**
- **40+ GB disk**
- Network bridge connected to your LAN (for cluster communication)

### Example setup:
Create four VMs:
```
ubuntu2404-vm ubuntu1
ubuntu2404-vm ubuntu2
ubuntu2404-vm ubuntu3
ubuntu2404-vm bastion
```

After installation, you can reference this discussion for details:
👉 [GitHub Discussion #272](https://github.com/community-scripts/ProxmoxVE/discussions/272)

---

## 🛠️ Step 2 — Prepare the Bastion Host

You’ll run **Ansible** and **Kubespray** from the bastion.

### SSH into bastion
```bash
ssh root@<bastion_ip>
```

### Update packages
```bash
apt update && apt upgrade -y
```

### Install dependencies
```bash
apt install -y python3-pip git curl
```

---

## 🐍 Step 3 — Set Up Python Virtual Environment

Create an isolated environment for Kubespray and Ansible.

```bash
python3 -m venv kube
source kube/bin/activate
```

---

## 📦 Step 4 — Clone and Configure Kubespray

Clone the Kubespray repository and install dependencies.

```bash
git clone https://github.com/kubernetes-sigs/kubespray.git kubespray
cd kubespray
pip install "ansible-core>=2.17.3,<2.18.0"
pip install -r requirements.txt
```

---

## 🧭 Step 5 — Configure the Inventory

Kubespray uses an **inventory file** to define your cluster nodes.

Create a new inventory:
```bash
cp -rfp inventory/sample inventory/mycluster
```

Edit `inventory/mycluster/inventory.ini`:
```ini
[all]
ubuntu1 ansible_host=192.168.1.11 ip=192.168.1.11
ubuntu2 ansible_host=192.168.1.12 ip=192.168.1.12
ubuntu3 ansible_host=192.168.1.13 ip=192.168.1.13

[kube_control_plane]
ubuntu1

[etcd]
ubuntu1

[kube_node]
ubuntu2
ubuntu3

[k8s_cluster:children]
kube_control_plane
kube_node
```

> 💡 **Tip:** Use `ansible_host` for the SSH target (the VM IP from Proxmox) and `ip` for the node’s internal IP (used by Kubernetes).

Test SSH connectivity:
```bash
ansible -i inventory/mycluster/inventory.ini all -m ping
```

---

## ⚙️ Step 6 — Deploy the Cluster

Run the main playbook:
```bash
ansible-playbook -i inventory/mycluster/inventory.ini cluster.yml -b
```

This may take 15–30 minutes depending on your resources.

---

## ✅ Step 7 — Verify Cluster

Once complete, SSH into the control plane node:

```bash
ssh ubuntu1
kubectl get nodes
kubectl get pods -A
```

You should see all 3 nodes in the **Ready** state.

---

## 🌐 Optional — Access the Cluster Remotely

You can copy the `admin.conf` file to your local machine:

```bash
scp ubuntu1:/etc/kubernetes/admin.conf ~/.kube/config
```

Then:
```bash
kubectl get nodes
```

---

## 🧩 Troubleshooting

- **SSH key errors:** Ensure your bastion has SSH access to all nodes (`~/.ssh/id_rsa`).
- **Python or pip issues:** Re-run in the virtual environment.
- **Connection timeout:** Verify Proxmox VM networking (bridge and IP assignment).

---

## 🧾 Summary

| Component | Purpose |
|------------|----------|
| **Proxmox Community Script** | Quickly deploy Ubuntu 24.04 VMs |
| **Bastion Host** | Runs Kubespray & Ansible |
| **Kubespray** | Automates Kubernetes installation |
| **Ansible** | Executes cluster provisioning |
| **Ubuntu 24.04** | Lightweight, LTS base OS |

---

Would you like me to include a **minimal working inventory.ini** (ready to paste and run with your 3 nodes + bastion example)?  
I can also add **CNI choice (Calico/Flannel)** and **post-install verification commands** if you want to extend it into a full deployment manual.