To expose Traefik on one IP using NodePort and route external traffic through it, follow this step-by-step setup. This is ideal when you don’t have a cloud provider or want to control traffic using your own router, Nginx reverse proxy, or something like Cloudflare Tunnel.

---

## ✅ Step-by-Step: Expose Traefik via NodePort and Use One External IP

---

### 1. Deploy Traefik via Helm with NodePort

If you're using Helm (recommended):

helm repo add traefik https://traefik.github.io/charts
helm repo update
Install Traefik with NodePort enabled:

helm install traefik traefik/traefik \
  --namespace kube-system --create-namespace \
  --set service.type=NodePort \
  --set service.ports.web.nodePort=30080 \
  --set service.ports.websecure.nodePort=30443
This will:

* Deploy Traefik in kube-system namespace.
* Expose:

  * HTTP on port 30080
  * HTTPS on port 30443

---

### 2. Get Node IP (External or Internal)

You need the IP of one of your Kubernetes nodes (ideally the one with internet access):

kubectl get nodes -o wide
Pick the EXTERNAL-IP (or INTERNAL-IP if behind NAT/router).

---

### 3. Set DNS or External Proxy to Point to That Node

Now route all traffic to that IP:

#### Option A: From Your Router (Port Forwarding)

* Forward port 80 → NODE_IP:30080
* Forward port 443 → NODE_IP:30443

#### Option B: With NGINX (if installed on a VM or external host)

server {
    listen 80;
    server_name your-domain.com;

    location / {
        proxy_pass http://NODE_IP:30080;
    }
}

server {
    listen 443 ssl;
    server_name your-domain.com;

    ssl_certificate /etc/nginx/ssl/fullchain.pem;
    ssl_certificate_key /etc/nginx/ssl/privkey.pem;

    location / {
        proxy_pass https://NODE_IP:30443;
    }
}
#### Option C: Cloudflare Tunnel (Optional)

If you want to expose Traefik securely without public IP, you can use a Cloudflare Tunnel:

cloudflared tunnel create traefik
cloudflared tunnel route dns traefik your-domain.com

# In config.yml
tunnel: traefik
credentials-file: /etc/cloudflared/traefik.json

ingress:
  - hostname: your-domain.com
    service: http://NODE_IP:30080
  - service: http_status:404
---

### 4. Deploy Ingress or IngressRoute

Here’s a basic Ingress example:

apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: whoami
  namespace: default
  annotations:
    traefik.ingress.kubernetes.io/router.entrypoints: web
spec:
  rules:
    - host: whoami.your-domain.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: whoami
                port:
                  number: 80
You can also use IngressRoute if you’re using CRDs.