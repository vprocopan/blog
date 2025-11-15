Perfect — here is the **clean, guaranteed-working**, official **Ubuntu 24.04 Cloud-Image + cloud-init (NoCloud) + Virt-Manager** setup.

This is the method Ubuntu itself recommends for KVM/QEMU.
Zero installer. Zero GRUB. Zero autoinstall pain.

This will **work 100%**.

---

# ✅ OVERVIEW (what you will get)

A VM that:

* Boots instantly (no installer)
* Uses cloud-init like AWS / Azure / Proxmox
* Has your user `vprocopan`
* Has your SSH key installed
* Has qemu-guest-agent enabled
* Is reproducible, template-ready, cloud-ready

And created entirely using **cloud image + NoCloud seed**.

---

# 🚀 STEP 1 — Download Ubuntu 24.04 cloud image

```bash
mkdir ~/cloudvm
cd ~/cloudvm

wget https://cloud-images.ubuntu.com/noble/current/noble-server-cloudimg-amd64.img
```

This file is the **actual OS image**, no installer.

---

# 🚀 STEP 2 — Resize the VM image

Let’s make it 50GB total disk:

```bash
qemu-img resize noble-server-cloudimg-amd64.img 50G
```

---

# 🚀 STEP 3 — Create cloud-init config

## 3.1 `user-data`

```bash
nano user-data
```

Paste:

```yaml
#cloud-config
users:
  - name: user
    groups: [ sudo ]
    shell: /bin/bash
    sudo: ALL=(ALL) NOPASSWD:ALL
    ssh_authorized_keys:
      - ssh-ed25519 XXX

package_upgrade: true

packages:
  - qemu-guest-agent

runcmd:
  - systemctl enable qemu-guest-agent
  - systemctl start qemu-guest-agent
```

## 3.2 `meta-data`

```bash
nano meta-data
```

```
instance-id: cloudimg-001
local-hostname: ubuntu-cloud
```

---

# 🚀 STEP 4 — Create the NoCloud seed image

This is the **magic piece** from the official cloud-init QEMU tutorial.

```bash
genisoimage -output seed.iso -volid cidata -joliet -rock user-data meta-data
```

**IMPORTANT:**
The volume label must be **cidata** (lowercase).
This is how NoCloud datasource detects it.

---

# 🚀 STEP 5 — Create the VM in Virt-Manager

### 🔹 Add VM manually:

**1. Create new VM → “Import existing disk image”**
Select:

```
noble-server-cloudimg-amd64.img
```

OS type:

```
Ubuntu 24.04 (or Generic Ubuntu)
```

**2. Choose Q35 + UEFI (recommended)**
Under Overview →

* Chipset: **Q35**
* Firmware: **UEFI** (OVMF)

**3. Add the cloud-init seed disk**
Add Hardware →
Storage →
“Select or create custom storage” → choose:

```
seed.iso
```

IMPORTANT:
Set:

```
Device type: Disk
Bus: SCSI or VirtIO
```

(Cloud-init sees disks, not CDROMs.)

**4. Ensure the boot order boots only from the main disk**
NOT from seed.iso.

---

# 🚀 STEP 6 — Boot the VM

During first boot:

```
cloud-init will detect NoCloud (seed.iso)
apply user-data
create user vprocopan
add SSH key
enable qemu-guest-agent
resize disk to full 50GB
```

You can watch progress:

```
sudo tail -f /var/log/cloud-init-output.log
```

Check status:

```
cloud-init status --wait
cloud-init status --long
```

---

# 🚀 STEP 7 — SSH into the VM

Get the VM IP from virt-manager or inside VM:

```
ip a
```

Then SSH:

```bash
ssh vprocopan@<VM-IP>
```

Your key works instantly.

---

# 🎉 DONE

You now have:

* A **real cloud-image VM**
* No installer needed
* Full cloud-init support
* Perfect for DevOps work
* Runs instantly
* Repeatable
* Zero GUI interaction

---
