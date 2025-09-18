OWASP Juice Shop is an intentionally vulnerable web application for security testing, CTFs, and learning.  
This guide explains how to deploy it **inside a Docker container** on **Debian/Ubuntu** — perfect for running in a **Proxmox VM**.

Requirements

- Proxmox VM running Debian 12 or Ubuntu 22.04+
- At least **1 vCPU**, **2 GB RAM**, **10 GB disk**
- Internet access for pulling Docker image

---

Step 1: Install Docker

Run the following commands inside your Debian/Ubuntu VM:

bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y docker.io docker-compose
sudo systemctl enable docker --now
Verify installation:

bash
Code kopieren
docker --version
docker-compose --version
🚀 Step 2: Run OWASP Juice Shop
Start the container in detached mode and enable auto-restart:

bash
Code kopieren
docker run -d \
  --restart unless-stopped \
  -p 3000:3000 \
  bkimminich/juice-shop
-d → Runs container in the background

--restart unless-stopped → Restarts container on reboot or crash

-p 3000:3000 → Maps container port 3000 to VM port 3000

Check that it’s running:

bash
Code kopieren
docker ps
You should see something like:

bash
Code kopieren
CONTAINER ID   IMAGE                       PORTS                  NAMES
abcd1234efgh   bkimminich/juice-shop       0.0.0.0:3000->3000/tcp awesome_juice
Step 3: Access the App
Open your browser and go to:

cpp
Code kopieren
http://<your-vm-ip>:3000
Example:

If your VM IP is 192.x.y.z, open http://192.x.y.z:3000

Managing the Container
Stop Juice Shop:

bash
Code kopieren
docker stop $(docker ps -q --filter ancestor=bkimminich/juice-shop)
Start Juice Shop again:

bash
Code kopieren
docker start $(docker ps -aq --filter ancestor=bkimminich/juice-shop)
View Logs:

bash
Code kopieren
docker logs -f $(docker ps -q --filter ancestor=bkimminich/juice-shop)
Security Note
Juice Shop is intentionally vulnerable.
Do not expose it to the internet without proper network isolation (VPN, firewall rules, or reverse proxy with access control).

Screenshots (Optional)
Add a screenshot of Juice Shop running in your browser for extra clarity.

Summary
This guide lets you deploy OWASP Juice Shop with:

Docker container (lightweight, isolated)

Auto-restart on reboot

Accessible from any device on your LAN

Perfect for security training, labs, and CTF practice inside your Proxmox homelab.

References
OWASP Juice Shop GitHub https://github.com/juice-shop/juice-shop

Docker Hub Image https://hub.docker.com/r/bkimminich/juice-shop
