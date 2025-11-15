Here is your **complete Ubuntu 24.04 Autoinstall (cloud-init) package**, customized for:

* **username:** `vprocopan`
* **SSH key:** your ed25519 key
* **disk size:** 50GB

This will install Ubuntu **fully automatically** — no questions asked.

Use it with **ubuntu-24.04-live-server.iso**.

---

# 📦 **FILES YOU NEED (copy-paste exactly)**

You need **two files** in the same folder:

---

# 1️⃣ **user-data** *(main autoinstall config)*

Create a file named:

```
user-data
```

Content:

```yaml
#cloud-config
autoinstall:
  version: 1
  identity:
    username: vprocopan
    hostname: ubuntu-server
    password: ""
  ssh:
    install-server: true
    authorized-keys:
      - ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIAGDn/awYr48lfHwN5PqoLOO5If2gy5LUgl1PfDzVssz vprocopan@fedora
  storage:
    layout:
      name: direct
    swap:
      size: 0
    config:
      - type: disk
        id: disk0
        match:
          size: 50G
        ptable: gpt
        wipe: superblock
        preserve: false
      - type: partition
        id: boot-partition
        device: disk0
        size: 512M
        flag: boot
      - type: partition
        id: root-partition
        device: disk0
        size: -1
      - type: format
        id: boot-format
        fstype: fat32
        volume: boot-partition
      - type: format
        id: root-format
        fstype: ext4
        volume: root-partition
      - type: mount
        path: /
        device: root-format
  packages:
    - qemu-guest-agent
    - net-tools
    - htop
  updates: security
  locale: en_US.UTF-8
  keyboard:
    layout: us
  apt:
    preserve_sources_list: false
  user-data:
    disable_root: true
  late-commands:
    - curtin in-target --target=/target systemctl enable qemu-guest-agent
    - curtin in-target --target=/target systemctl start qemu-guest-agent
```

✔ Creates **UEFI boot partition + ext4 root**
✔ Installs qemu-guest-agent
✔ Enables SSH with your key
✔ No root password
✔ Fully automatic installation
✔ Disk autoconfigured to **50GB** exactly

---

# 2️⃣ **meta-data**

Create a file named:

```
meta-data
```

Content:

```yaml
instance-id: ubuntu-autoinstall
local-hostname: ubuntu-server
```

---

# 📀 **How to Use These Files in Virt-Manager**

This is the easy part.

---

## 🟣 **STEP 1 — Create a seed ISO**

Install cloud-image-utils (if missing):

Fedora:

```bash
sudo dnf install cloud-image-utils genisoimage
```

Create the seed ISO:

```bash
genisoimage -output seed.iso -volid CIDATA -joliet -rock user-data meta-data
```

This generates:

```
seed.iso
```

---

## 🟣 **STEP 2 — Create the VM in Virt-Manager**

* OS: Ubuntu 24.04 Server ISO (`ubuntu-24.04-live-server-amd64.iso`)
* Machine: Q35
* Firmware: UEFI (OVMF)
* Disk: 50GB qcow2 (Virtio)
* CPU: host-passthrough, 4 cores
* RAM: 4GB

---

## 🟣 **STEP 3 — Add the seed ISO**

Before starting the VM:

🟡 Go to **Add Hardware → Storage → Select seed.iso → CDROM**
🟡 Set boot order:

1. Autoinstall seed ISO
2. Ubuntu installer ISO
3. Disk

---

## 🟣 **STEP 4 — Boot the VM**

Ubuntu installer will detect the `CIDATA` seed and immediately:

* Partition the disk
* Install Ubuntu
* Set username
* Enable SSH
* Add your key
* Install guest agent
* Auto reboot

No interaction needed.

---

## 🟣 **STEP 5 — First Login**

After install completes:

### From Virt-Manager console:

```bash
ssh vprocopan@<VM_IP>
```

Key-based login will work automatically.

---

# 🎉 Done

You now have a fully automatic **Ubuntu 24.04 autoinstall** configuration — no prompts, no manual steps.

---
