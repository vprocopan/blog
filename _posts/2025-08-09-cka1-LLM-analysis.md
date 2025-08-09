
# CKA Exam Preparation Notes

## What are services?

In the exam, you will be asked to set up a pod that can expose a service internally within the cluster.  
You are not asked to create an external LoadBalancer service or expose any port on the node.  
You should use the `services` definition to expose the pod internally, e.g. on port **443**.

> **Solution**  
> The solution for the exam is typically a single file that contains the YAML definition for the pod and the service.

```
---
apiVersion: v1
kind: Pod
metadata:
  name: frontend
spec:
  containers:
    - name: nginx
      image: nginx
      ports:
        - containerPort: 443
---
apiVersion: v1
kind: Service
metadata:
  name: frontend
spec:
  selector:
    app: frontend
  ports:
    - name: https
      port: 443
      targetPort: 443
```

## Where is the API server?

The API server is a **stateless** Kubernetes component.  
It does not need to run on a node.  
In a single‑node cluster, the API server is usually the same process as `kubelet` or `kubeadm`, running on the same machine.  
In a multi‑node cluster, the API server is typically a **deployment** or a **statefulset** that runs on a dedicated node.

**Key Points**  
* The API server **does not** run on a node, but it **may** run on a node in a single‑node installation.  
* In a multi‑node cluster, the API server is a **pod** running on a node.  
* The API server is **stateless** – it never needs to be restarted for data to be preserved.

## Exam structure and topics

**The exam is structured into the following key areas:**

1. **Services and DNS** – Creating and exposing services, DNS resolution inside the cluster, and the service's IP.
2. **Internal Load Balancing** – Implementing load balancing for services inside the cluster (NodePort, ClusterIP, LoadBalancer).
3. **Pod Disruption Budgets** – Enforcing limits on voluntary disruptions for pods.
4. **Node & Pod Security** – Managing RBAC, NetworkPolicy, SecurityContext, and NodeSecurityPolicy.
5. **Kubernetes Upgrades** – Upgrading a running cluster, rolling updates, and handling API deprecations.
6. **Kubelet TLS Bootstrapping** – Enabling secure communication between kubelet and API server.
7. **Quality of Service** – Understanding QoS classes and how they are determined.
8. **API access from Pods** – Using the in‑cluster configuration to access the API server.
9. **Exam Preparation** – General guidelines, resources, and best practices.

### Service and DNS

In a Kubernetes cluster, services provide **stable IP addresses** and DNS names.  
When you create a **Service** object, the API server assigns a unique IP address that can be used by other pods.  
The **kube-dns** or **coredns** pods resolve the service name to its IP address automatically.

**Key Points**  
* Service names are resolved by the DNS service.  
* Service IPs are stable until the service is deleted.  
* Services can be exposed to external traffic via LoadBalancer or NodePort.

### Internal Load Balancing

Internal load balancing can be achieved with **ClusterIP** or **NodePort** services.  
**ClusterIP** services are only reachable inside the cluster.  
**NodePort** services expose the service on a static port on each node.

```
apiVersion: v1
kind: Service
metadata:
  name: frontend
spec:
  selector:
    app: frontend
  type: ClusterIP
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
```

### Pod Disruption Budgets

A **Pod Disruption Budget** (PDB) limits the number of pods that can be voluntarily evicted.  
This ensures high availability even during maintenance or upgrades.

**Example**

```yaml
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: frontend-pdb
spec:
  minAvailable: 3
  selector:
    matchLabels:
      app: frontend
```

### Node & Pod Security

1. **RBAC** – Use RoleBasedAccessControl to restrict what users and service accounts can do.
2. **NetworkPolicy** – Limit traffic between pods and namespaces.
3. **SecurityContext** – Set runAsUser, runAsGroup, privileged, readOnlyRootFilesystem.
4. **NodeSecurityPolicy** – Enforce node-level security constraints.

#### Example: NetworkPolicy

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-web
spec:
  podSelector:
    matchLabels:
      app: frontend
  policyTypes:
  - Ingress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          role: web
    ports:
    - protocol: TCP
      port: 80
```

## Upgrade Kubernetes

> **NOTE**: The exam may ask you to upgrade a Kubernetes cluster while ensuring minimal downtime and preserving workloads.

### Basic steps

1. **Check the current version**  
   ```bash
   kubectl version --short
   ```

2. **Plan the upgrade** – Decide whether to upgrade the control plane first or the worker nodes first.  
   *Upgrade control plane → node upgrade* is a common pattern.

3. **Upgrade the API server** – Update the kube-apiserver image and restart the pod.  
4. **Upgrade the controller manager** – Update the kube-controller-manager image and restart the pod.  
5. **Upgrade the scheduler** – Update the kube-scheduler image and restart the pod.  
6. **Upgrade kubelet** – Download the new binary or update the image.  
7. **Upgrade kube-proxy** – Update the kube-proxy image on each node.  
8. **Upgrade etcd** – If you’re using external etcd, upgrade it separately.  
9. **Verify** – Check that all pods are running, and the API server is healthy.

### Common pitfalls

* **Incompatible client libraries** – The API server may reject requests from older client versions.  
* **Missing resources** – API objects may be removed or renamed in newer releases.  
* **Networking issues** – kube-proxy may misbehave if not updated correctly.  
* **RBAC changes** – New API groups or resources may be disallowed by existing roles.  

## Kubelet TLS Bootstrapping

If you’re running **kubeadm** in a single‑node cluster, you can enable the **kubelet TLS bootstrap** feature.  
This allows the kubelet to automatically obtain its client certificate from the API server.

1. **Set the environment variable**  
   ```bash
   export KUBELET_BOOTSTRAP_TOKEN="$(openssl rand -hex 16)"
   ```
2. **Create a token**  
   ```bash
   kubeadm token create --print-join-command
   ```
3. **Join the node** – Use the printed command to join the cluster.  
4. **Verify** – Ensure the kubelet is running with the correct certificate.

**Note**: This feature requires API server flag `--tls-cert-file` and `--tls-private-key-file`.

## Quality of Service

In Kubernetes, **Quality of Service (QoS)** is determined by:

* **Resource requests** – Minimum guaranteed resources.
* **Resource limits** – Maximum allowed resources.
* **Priority class** – Determines which pods get scheduled first during resource pressure.

### QoS Classes

| Class | Condition | Description |
|-------|-----------|-------------|
| **Guaranteed** | Requests = Limits for all resources | Highest priority. |
| **Burstable** | Requests < Limits for at least one resource | Medium priority. |
| **BestEffort** | No requests or limits | Lowest priority. |

## API access from Pods

Kubernetes provides an **in‑cluster configuration** that allows pods to authenticate to the API server using the service account token mounted in `/var/run/secrets/kubernetes.io/serviceaccount`.

```go
package main

import (
    "context"
    "fmt"
    "k8s.io/client-go/kubernetes"
    "k8s.io/client-go/rest"
)

func main() {
    config, err := rest.InClusterConfig()
    if err != nil {
        panic(err.Error())
    }
    clientset, err := kubernetes.NewForConfig(config)
    if err != nil {
        panic(err.Error())
    }

    pods, err := clientset.CoreV1().Pods("").List(context.TODO(), metav1.ListOptions{})
    if err != nil {
        panic(err.Error())
    }

    fmt.Printf("There are %d pods in the cluster\n", len(pods.Items))
}
```

> **IMPORTANT**: The pod **must** run inside the cluster and have the appropriate service account token.  
> If the token is missing or the pod does not have the necessary RBAC permissions, the call will fail.

## Exam Preparation

### General Guidelines

1. **Practice with `kubectl`** – Know how to create, delete, and inspect resources.  
2. **Understand YAML** – Be comfortable editing YAML files for resources.  
3. **Know the API groups** – Familiarize yourself with common API groups (`apps/v1`, `networking.k8s.io/v1`, etc.).  
4. **Use namespaces** – Isolate workloads and resources.  
5. **RBAC best practices** – Create roles and bind them to service accounts.  
6. **NetworkPolicy** – Practice writing policies to allow/deny traffic.  
7. **Upgrade testing** – Simulate upgrades in a lab environment.

### Resources

* Official Kubernetes documentation – https://kubernetes.io/docs/
* Kubeadm documentation – https://kubeadm.io/
* Kubernetes API reference – https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.27/
* Tutorials and labs – https://www.learnk8s.io/ or similar platforms.

## Frequently Asked Questions

| # | Question | Answer |
|---|----------|--------|
| 1 | What is the difference between ClusterIP and NodePort services? | ClusterIP is only accessible inside the cluster, whereas NodePort exposes the service on a static port on each node. |
| 2 | How many nodes must a cluster have for the API server to be considered stateless? | The API server is considered stateless even in a single‑node cluster; the key is that it does not store state locally. |
| 3 | How do you create a service that only accepts internal traffic? | Use `type: ClusterIP`. |
| 4 | What are the required environment variables for kubelet TLS bootstrapping? | `KUBELET_BOOTSTRAP_TOKEN` and the kubeadm token generated by `kubeadm token create`. |
| 5 | Which QoS class is the highest priority? | **Guaranteed**. |
| 6 | Which resources are affected by resource requests and limits? | CPU and memory, though other resources can be defined. |
| 7 | What does a Pod Disruption Budget do? | Limits the number of pods that can be voluntarily evicted during maintenance. |

## Test Cases

**Test Case 1 – Service and DNS**

* **Create** a `Service` with `ClusterIP` type and a `Pod` that selects it.  
* **Verify** DNS resolution: `nslookup frontend.default.svc.cluster.local`.  
* **Check** the assigned IP: `kubectl get svc frontend -o wide`.  

**Test Case 2 – Internal Load Balancing**

* **Create** a `Service` of type `ClusterIP` and another of type `NodePort`.  
* **Test** connectivity from another pod using the service name.  
* **Verify** that the NodePort service is accessible on the node's IP.  

**Test Case 3 – Pod Disruption Budget**

* **Define** a PDB with `minAvailable: 2`.  
* **Simulate** a node drain (`kubectl drain <node>`) and ensure at least 2 pods stay running.  

**Test Case 4 – Node Security**

* **Create** a `NetworkPolicy` that denies all inbound traffic.  
* **Add** a rule to allow traffic from a specific namespace or pod label.  
* **Verify** connectivity with `curl` or `nc` between pods.  

**Test Case 5 – Upgrade**

* **Deploy** a small cluster with at least one control plane node and one worker node.  
* **Upgrade** from `v1.23` to `v1.26` following the steps above.  
* **Confirm** that all workloads remain running and no API errors occur.  

**Test Case 6 – TLS Bootstrap**

* **Generate** a bootstrap token and create a kubeadm token.  
* **Join** a new node using the printed command.  
* **Verify** that the kubelet obtains a valid client certificate and starts successfully.  

---  

### Quick Reference – Commands

| Purpose | Command |
|---------|---------|
| Get cluster version | `kubectl version --short` |
| Create a service | `kubectl apply -f service.yaml` |
| Delete a service | `kubectl delete svc <name>` |
| Create a pod | `kubectl apply -f pod.yaml` |
| Delete a pod | `kubectl delete pod <name>` |
| View logs | `kubectl logs <pod>` |
| Check node status | `kubectl get nodes` |
| Verify kubelet status | `systemctl status kubelet` |
| Upgrade kubelet | `kubeadm upgrade node` |
| Upgrade control plane | `kubeadm upgrade apply <new-version>` |

---

> **Best Practice** – Always keep the cluster **backed up** before performing upgrades or major changes.  
> Use namespaces to isolate workloads and apply **RBAC** and **NetworkPolicy** to enforce security.  

---  

### Resources

* Official Kubernetes docs – https://kubernetes.io/docs/
* kubeadm documentation – https://kubeadm.io/
* API reference – https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.27/
* Learning platforms – e.g., https://www.learnk8s.io/, https://www.edx.org/, or similar.