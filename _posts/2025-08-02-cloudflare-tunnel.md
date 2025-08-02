Here’s how you can securely expose Code Server over HTTPS using Cloudflare Tunnel (cloudflared), step by step:

⸻

✅ Prerequisites
	•	You have a domain registered in Cloudflare (e.g., example.com).
	•	You have cloudflared installed.
	•	code-server is already installed and running on localhost:8080 or similar.
	•	You have a Cloudflare account and access to the dashboard.

⸻

🛠 Step 1: Authenticate cloudflared with Cloudflare

Run:

cloudflared tunnel login

This will open a browser to authenticate with Cloudflare and select the domain you own.

⸻

🛠 Step 2: Create a tunnel

cloudflared tunnel create code-server

This creates a new tunnel named code-server and outputs a Tunnel ID. It also creates credentials in:

~/.cloudflared/<TunnelID>.json


⸻

🛠 Step 3: Create a config file

Create the config file at: ~/.cloudflared/config.yml

tunnel: code-server
credentials-file: /home/YOUR_USERNAME/.cloudflared/<TunnelID>.json

ingress:
  - hostname: code.example.com
    service: http://localhost:8080
  - service: http_status:404

Replace:
	•	YOUR_USERNAME → your actual Linux username.
	•	code.example.com → your desired subdomain.
	•	localhost:8080 → your Code Server port.

⸻

🛠 Step 4: Add a DNS record (automatically)

Run this to start the tunnel and update DNS:

cloudflared tunnel route dns code-server code.example.com


⸻

🛠 Step 5: Start the tunnel

Now start the tunnel:

cloudflared tunnel run code-server

You can now access Code Server via https://code.example.com securely through Cloudflare Tunnel.

⸻

🚀 Optional: Run the tunnel as a systemd service

sudo cloudflared service install

This ensures it starts on boot.

⸻

