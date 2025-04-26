If you want to **merge a new `kubeconfig`** into your **existing** one, right?

Here’s the clean way:

---

✅ If you just have a new `kubeconfig.yaml` file and you want to **add it** to your current setup:

```
KUBECONFIG=~/.kube/config:/path/to/new/kubeconfig.yaml kubectl config view --flatten > /tmp/config && mv /tmp/config ~/.kube/config
```

**What’s happening here:**
- `KUBECONFIG=a:b` → temporarily tell `kubectl` to load both configs (`a` and `b`).
- `kubectl config view --flatten` → merges them **properly** into one clean config.
- Save that result over your current `~/.kube/config`.

---

✅ Alternative **(using `kubectl config` command)** if you just want to **manually add a new cluster, user, and context**:

```
kubectl config set-cluster <cluster-name> --server=<server-url> --certificate-authority=<path-to-ca-cert>
kubectl config set-credentials <user-name> --token=<token>
kubectl config set-context <context-name> --cluster=<cluster-name> --user=<user-name>
kubectl config use-context <context-name>
```

(But the first way is easier if you already have a ready kubeconfig file.)

---

**Quick Tip:**  
If you work with multiple clusters a lot, tools like [`kubectx`](https://github.com/ahmetb/kubectx) make switching between them super easy 🚀.

---
