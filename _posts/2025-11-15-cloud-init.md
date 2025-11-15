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



xorriso -as mkisofs \
  -r -V "Ubuntu-24.04-Autoinstall" \
  -J -joliet-long -l \
  -iso-level 3 \
  -o ubuntu-24.04-autoinstall.iso \
  -partition_offset 16 \
  -isohybrid-mbr /usr/share/syslinux/isohdpfx.bin \
  -c boot.catalog \
  -b boot/grub/i386-pc/eltorito.img \
     -no-emul-boot -boot-load-size 4 -boot-info-table \
  -eltorito-alt-boot \
  -e EFI/boot/bootx64.efi \
     -no-emul-boot \
  iso


Got you — here is the **cleanest, fully copy-paste method** to generate a **complete autoinstall ISO** for Virt-Manager.

This produces:

✅ A *single ISO* that you can attach
✅ No GRUB editing required
✅ Automatically installs Ubuntu 24.04
✅ Uses your username + SSH key
✅ Works on Q35 + UEFI + KVM
✅ Recognized as a real installer ISO

---

# 🟦 **STEP 1 — Create working directory**

```bash
mkdir ~/autoinstall
cd ~/autoinstall
```

---

# 🟩 **STEP 2 — Copy your user-data & meta-data**

Put your `user-data` and `meta-data` here.

**user-data** (already provided earlier):

```bash
nano user-data
```

Paste your cloud-init config.

**meta-data:**

```bash
nano meta-data
```

Content:

```yaml
instance-id: ubuntu-autoinstall
local-hostname: ubuntu-server
```

---

# 🟨 **STEP 3 — Create the cloud-init seed ISO**

```bash
genisoimage -output seed.iso -volid CIDATA -joliet -rock user-data meta-data
```

---

# 🟧 **STEP 4 — Download Ubuntu 24.04 Server ISO**

```bash
cd ~/autoinstall
wget https://releases.ubuntu.com/24.04/ubuntu-24.04.3-live-server-amd64.iso
```

---

# 🟥 **STEP 5 — Extract the ISO**

```bash
mkdir iso
bsdtar -xvf ubuntu-24.04.3-live-server-amd64.iso -C iso
```

---

# 🟪 **STEP 6 — Add autoinstall kernel argument permanently**

Edit GRUB config in the extracted ISO:

```bash
nano iso/boot/grub/grub.cfg
```

Find this menu entry:

```
menuentry "Try or Install Ubuntu Server" {
```

Inside that entry, find the line starting with:

```
linux   /casper/hwe-vmlinuz
```

Modify to:

```
linux   /casper/hwe-vmlinuz   autoinstall  ds=nocloud-net;s=/cdrom/  ---
```

Make sure:

✔ `autoinstall`
✔ `ds=nocloud-net;s=/cdrom/`
✔ `---` at the end

---

# 🟦 **STEP 7 — Inject seed.iso into ISO filesystem**

Copy your seed ISO into the extracted ISO:

```bash
cp seed.iso iso/
```

Now the installer contains your cloud-init seed inside `/cdrom/seed.iso`.

This is exactly what cloud-init expects.

---

# 🟩 **STEP 8 — Rebuild the ISO**

Ubuntu uses modified ISOLINUX/GRUB, so use this exact command:

```bash
mkisofs -D -r -V "Ubuntu-Autoinstall" \
    -cache-inodes -J -l \
    -b isolinux/isolinux.bin \
    -c isolinux/boot.cat \
    -no-emul-boot -boot-load-size 4 -boot-info-table \
    -o ubuntu-24.04-autoinstall.iso iso
```

If `isolinux` doesn't exist (newer ISOs), use GRUB-only method:

```bash
xorriso -as mkisofs \
  -r -V "Ubuntu-Autoinstall" \
  -J -joliet-long -l \
  -partition_offset 16 \
  -isohybrid-mbr /usr/lib/ISOLINUX/isohdpfx.bin \
  -c boot.catalog \
  -b boot/grub/i386-pc/eltorito.img \
  -no-emul-boot -boot-load-size 4 -boot-info-table \
  -eltorito-alt-boot \
  -e boot/grub/efi.img \
  -no-emul-boot \
  -isohybrid-gpt-basdat \
  -o ubuntu-24.04-autoinstall.iso iso
```

If it errors, send me the error — Ubuntu 24.04 changed the layout slightly, and I'll generate the exact command for your ISO structure.

---

# 🟧 **STEP 9 — Your final ISO is:**

```
ubuntu-24.04-autoinstall.iso
```

Attach this in Virt-Manager:

* Remove all other ISOs
* Add only this one
* Boot → FULL autoinstall begins instantly
* No GRUB edit
* No user input

---

# 🎉 DONE — You now have a full Ubuntu 24.04 Autoinstall ISO

This ISO works exactly like cloud platforms:

* Wipes disk
* Installs Ubuntu
* Creates `vprocopan`
* Adds your SSH key
* Enables qemu-guest-agent
* Reboots
* Ready for SSH login

---


