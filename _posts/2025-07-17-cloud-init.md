**`cloud-init`** is a widely used tool in Linux for **automatically configuring virtual machines or cloud instances at first boot**. It is commonly used by cloud providers like AWS, Azure, Google Cloud, DigitalOcean, and OpenStack to perform initialization tasks when a new instance is launched.

---

### 🔧 What It Does

`cloud-init` allows automatic configuration of things like:

* Setting the hostname
* Creating users and setting SSH keys
* Installing packages
* Running shell scripts
* Attaching volumes or configuring networks
* Writing files (e.g., configuration files)

---

### 📦 How It Works

1. **Data Sources**: It fetches instance metadata and configuration from sources like:

   * EC2 metadata service
   * ConfigDrive (used by OpenStack)
   * NoCloud (for local testing with ISO or disk)

2. **User-Data Script**: You can pass a **cloud-init script** (YAML or shell) when you create an instance. This is called **user data**.

3. **Execution Stages**:

   * `init`: Setup and detect data source
   * `config`: Apply configuration (users, files, etc.)
   * `final`: Run custom scripts

---

### 📄 Example `cloud-config` (YAML):

```yaml
#cloud-config
hostname: my-server
users:
  - name: vitalie
    sudo: ALL=(ALL) NOPASSWD:ALL
    ssh-authorized-keys:
      - ssh-rsa AAAAB3...

packages:
  - htop
  - git

runcmd:
  - echo "Welcome to cloud-init!" > /etc/motd
```

---

### 🛠 Common Use Cases

* Automating VM provisioning in CI/CD
* Creating reproducible server images
* Initializing Kubernetes nodes
* Bootstrapping cloud infrastructure

---

Great — since you're using **Proxmox**, here's how `cloud-init` works in that context:

---

### 🖥️ What is `cloud-init` in Proxmox?

In **Proxmox VE**, `cloud-init` is used to automatically configure **virtual machines (VMs)** (not containers) during their **first boot**, especially when you're cloning from a **cloud-init enabled template**.

It is ideal for creating **dynamic VMs with unique settings**, such as IP addresses, SSH keys, hostnames, and user credentials, from a single template.

---

### ✅ Typical Use Cases in Proxmox

* Automatically assign a static IP, DNS, and hostname
* Inject SSH public keys for login
* Set user passwords or disable password logins
* Run first-boot scripts (like package installs)

---

### 🧱 How It Works in Proxmox

1. **Create a Cloud-Init Ready VM Template**

   * Use a cloud-ready Linux image (e.g., Ubuntu Cloud, CentOS Cloud, Debian Cloud).
   * Add a cloud-init drive via the Proxmox GUI or CLI.

2. **Convert the VM to a Template**

   ```bash
   qm template <vmid>
   ```

3. **Clone New VMs from the Template**

   ```bash
   qm clone <template-id> <new-vm-id> --name new-vm-name
   ```

4. **Configure Cloud-Init Parameters**
   In the Proxmox UI (under the VM → **Cloud-Init** tab), or via CLI:

   ```bash
   qm set <vmid> --ciuser vitalie --sshkey ~/.ssh/id_rsa.pub \
     --ipconfig0 ip=192.168.1.100/24,gw=192.168.1.1
   ```

5. **Start the VM**

   ```bash
   qm start <vmid>
   ```

   `cloud-init` runs on boot and configures the VM according to your settings.

---

### 🔧 Requirements

* The guest OS **must have `cloud-init` pre-installed**.
* The VM should use **VirtIO** disk/network and **serial console** (for easier debug).
* The Proxmox VM must have a **cloud-init drive** added (`scsciX: cloudinit`).

---

### 🔍 Debugging Tips

* Log into the VM and check logs:

  ```bash
  sudo journalctl -u cloud-init
  sudo cloud-init status
  sudo cloud-init analyze
  ```

---


Here’s a **step-by-step guide** to create a **cloud-init enabled Ubuntu 22.04 template on Proxmox**, which you can use to clone VMs with unique config (IP, hostname, SSH keys, etc.):


---

## 🧰 Prerequisites

* Proxmox VE installed
* SSH access or console access to Proxmox host
* A Linux Bridge network in Proxmox (e.g. `vmbr0`)

---

## ✅ Step-by-Step: Ubuntu 22.04 Cloud-Init Template

### **1. Download Ubuntu Cloud Image**

```bash
wget https://cloud-images.ubuntu.com/jammy/current/jammy-server-cloudimg-amd64.img
```

You can rename it if you prefer:

```bash
mv jammy-server-cloudimg-amd64.img ubuntu-22.04-cloudinit.img
```

---

### **2. Create a New VM in Proxmox**

```bash
VMID=9000
qm create $VMID --name ubuntu-2204-cloudinit \
  --memory 2048 --cores 2 --net0 virtio,bridge=vmbr0 \
  --ostype l26 --serial0 socket --vga serial0
```

> `serial0` is important for cloud-init console access.

---

### **3. Import the Cloud Image Disk**

```bash
qm importdisk $VMID ubuntu-22.04-cloudinit.img local-lvm
```

> Replace `local-lvm` with your storage name if different.

---

### **4. Attach the Disk to the VM**

```bash
qm set $VMID --scsihw virtio-scsi-pci --scsi0 local-lvm:vm-$VMID-disk-0
qm set $VMID --boot c --bootdisk scsi0
```

---

### **5. Add Cloud-Init Drive**

```bash
qm set $VMID --ide2 local-lvm:cloudinit
```

Also set a default serial console:

```bash
qm set $VMID --serial0 socket --vga serial0
```

---

### **6. Convert to a Template**

```bash
qm template $VMID
```

Now this VM is a reusable **cloud-init template**.

---

## 🚀 Deploying from the Template

### **1. Clone the Template**

```bash
qm clone 9000 101 --name ubuntu-vm-01
```

---

### **2. Set Cloud-Init Options**

```bash
qm set 101 --ciuser vitalie --sshkey ~/.ssh/id_rsa.pub
qm set 101 --ipconfig0 ip=192.168.1.101/24,gw=192.168.1.1
```

> You can also do this from Proxmox UI: select the VM → **Cloud-Init** tab.

---

### **3. Start the VM**

```bash
qm start 101
```

---

### ✅ VM will now:

* Set hostname automatically
* Use static IP or DHCP
* Inject your SSH key
* Set login user (e.g., `vitalie`)

---

## 🧪 Test Login

```bash
ssh vitalie@192.168.1.101
```

---

## 🛠️ Useful Commands Inside VM

```bash
# Check cloud-init status
cloud-init status

# View logs
journalctl -u cloud-init

# Analyze boot performance
cloud-init analyze
```

---

