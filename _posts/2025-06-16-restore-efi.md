To repair the EFI (Extensible Firmware Interface) system partition on a drive, you'll typically need a live Linux or Windows recovery environment, depending on your operating system. Below are step-by-step guides for both **Windows** and **Linux** systems.

---

## 🪟 **For Windows Systems**

### 🔧 Requirements:

* Windows installation media (USB/DVD)
* Admin access to recovery environment

### ✅ Steps:

1. **Boot into Windows Recovery Environment (WinRE)**:

   * Boot from Windows installation media.
   * Choose your language, then click **"Repair your computer"**.
   * Select **Troubleshoot > Advanced Options > Command Prompt**.

2. **Find your EFI partition**:

   ```bash
   diskpart
   list disk
   select disk 0             # (or your OS drive)
   list partition
   select partition X        # (usually ~100MB, type: EFI)
   assign letter=s
   exit
   ```

3. **Rebuild the EFI boot files**:

   * Run this from the command prompt:

     ```cmd
     bcdboot C:\Windows /s S: /f UEFI
     ```
   * This copies boot files to the EFI system partition and recreates the BCD store.

4. **Reboot**:

   ```cmd
   exit
   ```

   Remove installation media and reboot your system.

---

## 🐧 **For Linux Systems (e.g., GRUB on Ubuntu/Debian)**

### 🔧 Requirements:

* Live USB of your Linux distro
* Root access in chroot environment

### ✅ Steps:

1. **Boot into Live USB** and open a terminal.

2. **Identify partitions**:

   ```bash
   sudo fdisk -l
   ```

   * Note your EFI partition (usually `/dev/sdX1`, FAT32, \~100–500MB)
   * Note your Linux root partition (e.g., `/dev/sdX2`)

3. **Mount and chroot into your system**:

   ```bash
   sudo mount /dev/sdX2 /mnt
   sudo mount /dev/sdX1 /mnt/boot/efi
   sudo mount --bind /dev /mnt/dev
   sudo mount --bind /proc /mnt/proc
   sudo mount --bind /sys /mnt/sys
   sudo chroot /mnt
   ```

4. **Reinstall GRUB and update config**:

   ```bash
   grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=GRUB
   update-grub
   ```

5. **Exit and reboot**:

   ```bash
   exit
   sudo umount -R /mnt
   sudo reboot
   ```

---

## 🧠 Notes:

* Make sure **UEFI** is enabled in BIOS settings.
* On Windows, `bcdboot` repairs both the BCD and EFI files.
* On Linux, `grub-install` + `update-grub` restores bootloader in EFI.

---
