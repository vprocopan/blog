### 🧠 What is eBPF?

**eBPF (extended Berkeley Packet Filter)** is a powerful Linux kernel technology that allows **safe, sandboxed programs to run inside the kernel** — without modifying kernel code or loading unsafe modules.

It effectively allows you to:

> 🔍 **Observe, control, and optimize** kernel-level behavior **dynamically** and **safely**.

---

## 🔧 What eBPF Can Do

| Use Case             | Example                                                        |
| -------------------- | -------------------------------------------------------------- |
| 🔎 **Observability** | Track network calls, CPU/memory usage, syscall latencies       |
| 🔒 **Security**      | Monitor or block suspicious system calls, container activity   |
| 🌐 **Networking**    | Build firewalls, traffic shaping, load balancers (e.g. Cilium) |
| ⚙️ **Profiling**     | Live CPU flamegraphs, stack traces, I/O bottlenecks            |
| 🧪 **Tracing**       | Distributed tracing with low overhead (e.g. Coroot, Pixie)     |

---

## 📦 How It Works (Simplified)

1. **You write a tiny program** (in C or use a tool like bpftrace).
2. It's **compiled into eBPF bytecode**.
3. The kernel **verifies** the code (ensures it's safe).
4. It's **loaded into the kernel** and hooked to an event (e.g., network packet, syscall, tracepoint).
5. It runs in a sandboxed VM **inside the kernel** with near-zero overhead.

---

## 🔐 Why It's Safe

* eBPF code is **verified** before execution.
* It runs in a **sandbox** (no memory access outside bounds).
* It **cannot crash the kernel**.

---

## 🔥 eBPF Power Examples

* `bcc` / `bpftrace` → Observe kernel-level events (like strace++ on steroids)
* **Pixie** / **Coroot** → Use eBPF to trace services and monitor latency without instrumenting code
* **Cilium** → An eBPF-powered networking and security layer for Kubernetes

---

## 📌 Summary

> **eBPF = kernel superpower** for visibility, security, and performance — used by modern tools like Coroot, Pixie, Cilium, and more.
