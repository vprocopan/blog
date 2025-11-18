If you want **apt to update ONLY the Jenkins package** (and not upgrade the whole system), use:

---

## ✅ **1. Update package index**

You must always update sources first:

```bash
sudo apt update
```

---

## ✅ **2. Upgrade ONLY the Jenkins package**

```bash
sudo apt install --only-upgrade jenkins
```

This will:

* NOT install new packages
* NOT update anything else
* ONLY upgrade the `jenkins` package if an update exists

---

## 🔍 Check version afterwards:

```bash
dpkg -l | grep jenkins
```

---

## 📝 If Jenkins is installed via repo (recommended)

Make sure your `/etc/apt/sources.list.d/jenkins.list` is correct:

```bash
deb https://pkg.jenkins.io/debian-stable binary/
```

Then run the upgrade command above.

---

## ⛔ If you want to prevent other packages from upgrading

You can hold the rest:

```bash
sudo apt-mark hold '*'
sudo apt-mark unhold jenkins
```

But normally unnecessary — the `--only-upgrade` flag is enough.

---
