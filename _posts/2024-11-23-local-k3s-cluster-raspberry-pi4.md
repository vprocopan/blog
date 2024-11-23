how to make k3s home cluster with 4 nodes on raspberry pi4

Setting up a **K3s home cluster** with 4 nodes on Raspberry Pi 4 can provide you with a lightweight Kubernetes cluster for experimentation or personal use. Below is a step-by-step guide:

**Prerequisites**

1. **Hardware:**

• 4 Raspberry Pi 4 boards with at least 2GB of RAM each.

• microSD cards (16GB or more for each Raspberry Pi).

• Power supply and network connectivity (Ethernet is preferred).

2. **Operating System:**

• Use a 64-bit OS like Raspberry Pi OS Lite (64-bit) or Ubuntu Server 22.04 for ARM.

3. **Networking:**

• Static IPs or DHCP reservations for each Raspberry Pi.

• Hostnames assigned (e.g., k3s-master, k3s-node1, k3s-node2, k3s-node3).

4. **Prepare the System:**

• Update each Raspberry Pi:

sudo apt update && sudo apt upgrade -y

• Install curl and iptables (if not already installed):

sudo apt install -y curl iptables

5. **SSH Access:**

• Ensure SSH is enabled for remote management.

**Step 1: Install K3s on the Master Node**

1. **Choose the Master Node:** Assign one Raspberry Pi as the control plane node.
  
2. **Install K3s (Master):**
  

Run the following command to install K3s on the master node:

curl -sfL https://get.k3s.io | INSTALL_K3S_EXEC="--write-kubeconfig-mode=644 --node-name=k3s-master" sh -

• This installs K3s and sets it as the control plane.

• The --write-kubeconfig-mode=644 ensures the kubeconfig file is readable for other users.

3. **Retrieve the Node Token:**

The node token is required to join worker nodes. Retrieve it using:

sudo cat /var/lib/rancher/k3s/server/node-token

4. **Note the Master IP Address:**

Find the IP address of the master node (e.g., 192.168.1.100).

**Step 2: Install K3s on Worker Nodes**

1. **Prepare Each Worker Node:**

SSH into each worker node and run:

curl -sfL https://get.k3s.io | K3S_URL=https://<MASTER_IP>:6443 K3S_TOKEN=<NODE_TOKEN> sh -

Replace:

• <MASTER_IP> with the IP address of the master node.

• <NODE_TOKEN> with the token from the master node.

2. **Verify Node Registration:**

After a few seconds, verify that the worker node is connected to the master node:

kubectl get nodes

You should see all nodes listed (master and workers).

**Step 3: Configure Networking**

K3s uses Flannel as the default networking backend, which works out of the box. However, you can ensure connectivity by:

1. **Open Required Ports:**

Ensure the following ports are open between nodes:

• TCP: 6443 (API server)

• UDP: 8472 (Flannel VXLAN traffic)

• UDP/TCP: 10250-10255 (kubelet and metrics server)

2. **Optional: Configure Static IPs:**

Assign static IPs or use DHCP reservations on your router.

**Step 4: Optional Configurations**

1. **Enable** kubectl **on the Master Node:**

Install kubectl (if not already installed):

sudo apt install -y kubectl

Ensure the kubeconfig is accessible:

export KUBECONFIG=/etc/rancher/k3s/k3s.yaml

2. **Add a Load Balancer (Optional):**

Use **Traefik** (installed by default in K3s) or another ingress controller for load balancing.

3. **Deploy a Test Application:**

Verify your cluster with a test deployment:

kubectl create deployment nginx --image=nginx

kubectl expose deployment nginx --type=NodePort --port=80

Access the application at http://<NODE_IP>:<PORT>.

**Step 5: Monitor and Manage**

1. **Monitor Resources:**

Use commands like:

kubectl get pods -A

kubectl top nodes

2. **Upgrade K3s:**

Periodically update K3s with:

curl -sfL https://get.k3s.io | sh -

**Cluster Topology Example**

**Node Name** **Role** **IP Address**

k3s-master Control Plane 192.168.1.100

k3s-node1 Worker 192.168.1.101

k3s-node2 Worker 192.168.1.102

k3s-node3 Worker 192.168.1.103

---

###### Traefik

**What is Traefik?**

**Traefik** is a modern, cloud-native reverse proxy and load balancer designed for ease of deployment and dynamic service discovery. It integrates seamlessly with containerized environments and orchestrators like Kubernetes, Docker, and others.

In the context of Kubernetes:

• **Ingress Controller**: Traefik serves as an ingress controller to route external HTTP(S) traffic into the Kubernetes cluster based on rules.

• **Dynamic Configuration**: Traefik dynamically discovers and configures routes as services are deployed in Kubernetes.

• **TLS Support**: It has built-in Let’s Encrypt support for automatic SSL/TLS certificate management.

**Key Features of Traefik**

1. **Dynamic Routing**: Automatically detects and routes traffic to your services.
  
2. **Load Balancing**: Balances traffic across service replicas.
  
3. **TLS Termination**: Handles HTTPS and manages certificates automatically.
  
4. **Middleware**: Includes features like rate limiting, retries, and custom headers.
  
5. **Monitoring**: Provides metrics for Prometheus and visual dashboards.
  

**How to Configure Traefik in Kubernetes**

Traefik can be set up in Kubernetes as an **Ingress Controller**. Here’s how to do it:

**Step 1: Install Traefik Using Helm**

Helm is the easiest way to deploy Traefik. If you don’t have Helm installed, [install Helm](https://helm.sh/docs/intro/install/).

1. **Add the Traefik Helm Repository**:

helm repo add traefik https://traefik.github.io/charts

helm repo update

2. **Install Traefik**:

Deploy Traefik in your Kubernetes cluster using:

helm install traefik traefik/traefik --namespace traefik --create-namespace

This will:

• Create a traefik namespace.

• Install Traefik with default settings.

3. **Verify the Installation**:

Check if Traefik is running:

kubectl get pods -n traefik

You should see the Traefik pod running.

**Step 2: Expose Traefik to External Traffic**

Traefik can be exposed to the outside world via a **LoadBalancer** or **NodePort** service.

1. **Default Service**:

The default Helm chart exposes Traefik via a LoadBalancer. You can verify with:

kubectl get svc -n traefik

Example output:

NAME      TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)

traefik   LoadBalancer   10.152.183.254   192.168.1.200  80:31568/TCP,443:31668/TCP

2. **Access the Dashboard (Optional)**:

Traefik includes a dashboard for monitoring:

• Enable it by editing values.yaml or passing custom Helm values during installation:

ports:

  web:

    expose: true

    exposedPort: 80

  websecure:

    expose: true

    exposedPort: 443

additionalArguments:

  - "--api.insecure=true"

• Access the dashboard at http://<LoadBalancer_IP>:8080.

**Step 3: Configure Ingress Resources**

Traefik uses **Ingress** or **IngressRoute** resources to route traffic to services.

**Example: Basic Ingress for a Service**

1. Deploy a test service (e.g., NGINX):

kubectl create deployment nginx --image=nginx

kubectl expose deployment nginx --port=80 --type=ClusterIP

2. Create an Ingress resource:

apiVersion: networking.k8s.io/v1

kind: Ingress

metadata:

  name: nginx-ingress

  namespace: default

  annotations:

    traefik.ingress.kubernetes.io/router.entrypoints: web

spec:

  rules:

  - host: nginx.local

    http:

      paths:

      - path: /

        pathType: Prefix

        backend:

          service:

            name: nginx

            port:

              number: 80

3. Apply the Ingress resource:

kubectl apply -f nginx-ingress.yaml

4. Test it:

• Add nginx.local to your /etc/hosts file, pointing to the Traefik external IP.

• Access it at http://nginx.local.

**Step 4: Enable HTTPS (TLS)**

Traefik supports automatic TLS using Let’s Encrypt or custom certificates.

**Enable Let’s Encrypt in values.yaml:**

Add the following configuration:

additionalArguments:

  - "--certificatesresolvers.default.acme.tlschallenge=true"

  - "--certificatesresolvers.default.acme.email=your-email@example.com"

  - "--certificatesresolvers.default.acme.storage=/data/acme.json"

volumes:

  - name: acme-storage

    persistentVolumeClaim:

      claimName: acme-pvc

Traefik will automatically handle SSL certificates for ingress resources that specify:

annotations:

  cert-manager.io/issuer: "letsencrypt"

**Step 5: Advanced Configuration**

1. **Middlewares**:

Traefik middlewares allow features like rate limiting or redirects:

apiVersion: traefik.containo.us/v1alpha1

kind: Middleware

metadata:

  name: redirect-to-https

spec:

  redirectScheme:

    scheme: https

    permanent: true

2. **Custom Routes**:

Use IngressRoute for fine-grained control:

apiVersion: traefik.containo.us/v1alpha1

kind: IngressRoute

metadata:

  name: nginx-ingressroute

spec:

  entryPoints:

    - websecure

  routes:

  - match: Host(`nginx.local`)

    kind: Rule

    services:

    - name: nginx

      port: 80

**Step 6: Monitor Traefik**

1. Access metrics for Prometheus or Grafana:

• Enable Prometheus in values.yaml:

metrics:

  prometheus:

    enabled: true

• Use Traefik dashboards in Grafana.

2. Logs:

• Check logs for debugging:

kubectl logs -n traefik <traefik-pod-name>

**Conclusion**

With Traefik configured, you now have a robust ingress controller for your Kubernetes cluster. It dynamically discovers services, provides SSL termination, and allows easy configuration through Ingress and IngressRoute resources.