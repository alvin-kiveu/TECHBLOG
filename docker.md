# 🚢 Docker Installation Guide

This guide will help you install and set up Docker on your system so you can start building, shipping, and running containers.

---

## 📌 Requirements

- A 64-bit version of Ubuntu (20.04 or later recommended)
- A user account with `sudo` privileges
- Internet access


https://docs.docker.com/engine/install/ubuntu/

portaine

https://docs.portainer.io/start/install-ce/server/docker/linux


sudo apt update && sudo apt install docker.io -y
sudo systemctl enable docker
sudo systemctl start docker
✅ Step 2: Create a Docker Volume for Portainer
bash
Copy
Edit
docker volume create portainer_data
✅ Step 3: Run Portainer Container
Use this command to start Portainer with access to the Docker socket:

bash
Copy
Edit
docker run -d \
  -p 8000:8000 \
  -p 9443:9443 \
  --name portainer \
  --restart=always \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v portainer_data:/data \
  portainer/portainer-ce:latest
✅ Step 4: Access Portainer Web UI
Open your browser and go to:

arduino
Copy
Edit
https://your-server-ip:9443
You will be prompted to create an admin user.

Then, choose Docker Standalone environment to manage the local Docker instance.

✅ Optional: Install Portainer as a systemd Service
If you prefer to manage Portainer like a service, let me know and I can help create a systemd unit file for it.

⚙️ Ports Summary
9443: Portainer HTTPS UI (main one you use)

8000: Portainer agent tunnel (used for remote environments)

Let me know if you want to:

Expose Portainer over HTTP (9000) instead of HTTPS

Set up a reverse proxy (like NGINX or Caddy)

Secure with a Let's Encrypt SSL certificate

Best regards,
Alvin Kiveu







You said:
am geting Client sent an HTTP request to an HTTPS server.


ChatGPT said:
The error message:

"Client sent an HTTP request to an HTTPS server."

means you're trying to access Portainer over HTTP, but it's only listening on HTTPS (port 9443).

✅ Solution Options
✅ Option 1: Access Portainer Using HTTPS (Recommended)
If you're running this command:

bash
Copy
Edit
docker run -d \
  -p 8000:8000 \
  -p 9443:9443 \
  --name portainer \
  --restart=always \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v portainer_data:/data \
  portainer/portainer-ce:latest
Then use this URL in your browser:

arduino
Copy
Edit
https://your-server-ip:9443
🔒 Note: If you just go to http://your-server-ip:9443, it will trigger that error.

✅ Option 2: Run Portainer on HTTP (Port 9000)
If you don’t want to use HTTPS and SSL (for testing or internal use), stop and remove the container:

bash
Copy
Edit
docker stop portainer && docker rm portainer
Then re-run it with port 9000:

bash
Copy
Edit
docker run -d \
  -p 9000:9000 \
  --name portainer \
  --restart=always \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v portainer_data:/data \
  portainer/portainer-ce:latest
Now access it via:

arduino
Copy
Edit
http://your-server-ip:9000

