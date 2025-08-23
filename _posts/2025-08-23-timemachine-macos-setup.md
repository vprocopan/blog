To set up a Debian-based NAS that works well with macOS clients. I’ll break it down into steps:

---

## 1. Base Setup on Debian

1. Install Debian (stable recommended).
2. Update the system:

   ```bash
   sudo apt update && sudo apt upgrade -y
   ```
3. Install useful tools:

   ```bash
   sudo apt install vim htop curl wget net-tools avahi-daemon -y
   ```

---

## 2. Storage Preparation

1. Identify your disks:

   ```bash
   lsblk
   ```
2. Partition & format (e.g., Btrfs or ext4):

   ```bash
   sudo mkfs.btrfs /dev/sdX
   ```
3. Mount permanently by editing `/etc/fstab`.

---

## 3. File Sharing Options for macOS

macOS supports **SMB** and **AFP**. AFP is deprecated, so **SMB (via Samba)** is the best option.

### Install Samba

```bash
sudo apt install samba -y
```

### Configure

Edit `/etc/samba/smb.conf` and add a share:

```ini
[macshare]
   path = /srv/nas/macshare
   browseable = yes
   read only = no
   guest ok = no
   create mask = 0664
   directory mask = 0775
```

Create directory & set permissions:

```bash
sudo mkdir -p /srv/nas/macshare
sudo chown -R $USER:$USER /srv/nas/macshare
```

Add Samba user:

```bash
sudo smbpasswd -a yourusername
```

Restart Samba:

```bash
sudo systemctl restart smbd
```

---

## 4. Bonjour (Avahi) for Easy macOS Discovery

Install Avahi:

```bash
sudo apt install avahi-daemon avahi-discover -y
```

This allows your NAS to appear automatically in Finder under “Network”.

---

## 5. (Optional) Time Machine Support

macOS can back up to SMB shares if flagged as a Time Machine volume.

Add this inside your Samba config:

```ini
[TimeMachine]
   path = /srv/nas/timemachine
   valid users = yourusername
   read only = no
   vfs objects = fruit streams_xattr
   fruit:time machine = yes
   fruit:metadata = stream
```

Then:

```bash
sudo mkdir -p /srv/nas/timemachine
sudo chown -R yourusername:yourusername /srv/nas/timemachine
sudo systemctl restart smbd
```

---

## 6. Access from macOS

* In Finder: `⌘ + K` → `smb://<your-nas-ip>/macshare`
* Or find it under **Network** if Avahi works.

---
