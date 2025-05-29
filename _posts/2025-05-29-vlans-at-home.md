Here's a visualization of your home network VLAN setup using the TP-Link TL-SG116E switch:

* **Router** connects to Port 1 (Untagged VLAN 10)
* **PC and NAS** are in **VLAN 10 (Trusted)**
* **Smart TV and IP Camera** are in **VLAN 20 (IoT)**
* **Guest Laptop** is in **VLAN 30 (Guest)**

Each VLAN is isolated from the others by the switch. Let me know if you'd like a config table export or wiring diagram with port numbers!

![vlans](/assets/images/vlan.png)

The **TP-Link TL-SG116E** is a **smart unmanaged switch** that supports **802.1Q VLAN tagging**, **port-based VLANs**, and **PVID settings**, making it suitable for basic VLAN setups even without a VLAN-aware router.

---

## 🧩 Your Setup Plan (with TL-SG116E and a non-VLAN router)

### 🎯 Goal: Isolate devices using VLANs

* VLAN 10: Main trusted network (with internet)
* VLAN 20: IoT devices (no access to VLAN 10, optionally no internet)
* VLAN 30: Guests (isolated from all others)

---

## 🛠️ Step-by-Step: VLAN Setup on TL-SG116E

### 1. **Login to the Switch**

1. Connect a PC directly to the switch.
2. Use TP-Link's **Easy Smart Configuration Utility** or browser (IP is usually `192.168.0.1` or `192.168.0.100`).
3. Login (default: admin / admin).

---

### 2. **Create VLANs**

Go to **VLAN > 802.1Q VLAN > VLAN Config**:

| VLAN ID | VLAN Name | Member Ports |
| ------- | --------- | ------------ |
| 10      | Trusted   | 1, 2, 3      |
| 20      | IoT       | 1, 4, 5      |
| 30      | Guest     | 1, 6         |

> Port 1 is the **uplink to your router** — it must belong to all VLANs if you want them to reach the router (limited functionality unless router supports VLANs).

---

### 3. **Set Port VLAN Modes (Tag/Untag)**

Go to **VLAN > 802.1Q VLAN > VLAN Membership**:

* **Port 1** (Router): Set as **Untagged on VLAN 10 only** (assuming router can’t handle tags).
* **Ports 2–3** (Trusted): Untagged on VLAN 10.
* **Ports 4–5** (IoT): Untagged on VLAN 20.
* **Port 6** (Guest): Untagged on VLAN 30.

You can only assign **1 untagged VLAN per port** — this becomes the **PVID**.

---

### 4. **Set PVIDs (Port VLAN IDs)**

Go to **VLAN > 802.1Q PVID Setting**:

| Port | PVID |
| ---- | ---- |
| 1    | 10   |
| 2–3  | 10   |
| 4–5  | 20   |
| 6    | 30   |

This ensures each port sends/receives untagged packets **associated with the correct VLAN**.

---

## 🔐 VLAN Isolation Behavior

* **Devices on VLAN 10 (Ports 2–3)** can access the internet (via Port 1 → router).
* **Devices on VLAN 20 (Ports 4–5)** are isolated at Layer 2 — they cannot talk to VLAN 10.
* **Devices on VLAN 30 (Port 6)** are isolated too.

⚠️ **All VLANs share the same subnet** since the router doesn’t support VLAN interfaces — so while the switch isolates traffic, the router may bridge it back if you connect all VLANs to it.

---

## 🧠 Tips & Workarounds

### ✅ To Block Internet for IoT or Guest Devices:

* Do **not connect Port 1 (router)** to VLAN 20 or VLAN 30.
* Or use static IPs and don’t configure a gateway on those devices.

### ✅ To Provide Internet to Multiple VLANs (Partially):

You’ll need a **VLAN-aware router**, or:

* Use **multiple routers**, one per VLAN, plugged into different VLAN access ports.
* Or **set static IPs and use the same gateway** (router won’t segment them, but switch will isolate traffic locally).

---

## 🛜 Wi-Fi VLAN?

Only possible if your **Access Point supports VLAN-tagged SSIDs** (e.g., TP-Link Omada, Ubiquiti). Otherwise, Wi-Fi clients will be in the router's flat network.

---

## 🚦 Final Thoughts

Your TL-SG116E **can enforce VLAN separation at Layer 2**, even if your router is VLAN-blind. That’s great for:

* Reducing attack surface (IoT → no access to PC/NAS)
* Guest isolation
* Testing VLAN behavior

If you want full routing/firewall control between VLANs, a **VLAN-aware router** (like OpenWrt or pfSense) is the natural next step.


