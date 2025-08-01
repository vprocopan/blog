This command creates a **full system backup** using `tar`, with special options to preserve ownership, ACLs, and extended attributes. Let’s break it down:

---

## 🧾 Full Command

```bash
tar --numeric-owner --xattrs --acls -cvpzf /mnt/backup/system.tar.gz \
  --exclude=/mnt/backup --exclude=/proc --exclude=/sys \
  --exclude=/dev --exclude=/run /
```

---

## 🧩 Option-by-option breakdown:

|Option|Meaning|
|---|---|
|`tar`|The command-line archiving tool|
|`--numeric-owner`|Save numeric user/group IDs instead of names (important for restoring on different systems)|
|`--xattrs`|Include extended file attributes (like SELinux labels or user-defined metadata)|
|`--acls`|Include Access Control Lists (fine-grained permission info)|
|`-c`|Create a new archive|
|`-v`|Verbose — show files being processed|
|`-p`|Preserve file permissions|
|`-z`|Compress the archive using gzip|
|`-f /mnt/backup/system.tar.gz`|Output file path for the archive|

---

## 🚫 Excluded Paths

These are excluded because they are **dynamic, pseudo, or would cause issues** if archived:

|Path|Reason for Exclusion|
|---|---|
|`/mnt/backup`|Prevent backing up the archive itself recursively|
|`/proc`|Virtual filesystem — no real files to back up|
|`/sys`|Kernel info — also virtual and dynamic|
|`/dev`|Device files (sockets, pipes, etc.)|
|`/run`|Runtime system files (e.g., PID files)|

---

## 🗂️ Target

- The final `/` means: archive the entire root filesystem.
    
- It creates: `/mnt/backup/system.tar.gz`
    

---

## 🧠 Usage Notes

- Run as **root** (`sudo`) to ensure access to all files.
    
- You can later restore with:
    

```bash
sudo tar --xattrs --acls -xvpzf system.tar.gz -C /
```

> Make sure to boot into rescue or live mode before restoring the root FS.

---

