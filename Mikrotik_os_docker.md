MikroTik RouterOS Docker image using Docker Compose. Since your configuration exposes Winbox, SSH, the MikroTik API, OpenVPN, L2TP, and WireGuard, follow these steps on your Linux VPS.

Important: This image runs RouterOS in a containerized environment; it is not the same as running a physical MikroTik router. VPN functionality and network access depend on the image, host networking, and RouterOS configuration.

1. Install Docker and Docker Compose

On Ubuntu or Debian, run:

sudo apt update
sudo apt install -y docker.io docker-compose-v2
sudo systemctl enable --now docker

If your distribution does not provide docker-compose-v2, install Docker Compose using the official Docker installation instructions instead.

2. Create the Docker Compose file

Run:

mkdir -p ~/mikrotik
cd ~/mikrotik
nano docker-compose.yml

Paste this configuration:


services:
  routeros:
    container_name: mikrotik
    image: cnsoluciones/mikrotik:7.16.2
    restart: unless-stopped
    privileged: true

    ports:
      - "21:21/tcp"           # FTP
      - "2222:22/tcp"         # SSH
      - "23:23/tcp"           # Telnet
      - "8080:80/tcp"         # HTTP
      - "8443:443/tcp"        # HTTPS
      - "1194:1194/udp"       # OpenVPN UDP
      - "1194:1194/tcp"       # OpenVPN TCP, if configured
      - "1450:1450/udp"       # L2TP
      - "8291:8291/tcp"       # Winbox
      - "8728:8728/tcp"       # MikroTik API
      - "8729:8729/tcp"       # MikroTik API-SSL
      - "13231:13231/udp"     # WireGuard

    cap_add:
      - NET_ADMIN

    devices:
      - /dev/net/tun:/dev/net/tun

    tty: true
    stdin_open: true
3. Pull the image and start MikroTik

Run these commands from the directory containing docker-compose.yml:

sudo docker pull cnsoluciones/mikrotik:7.16.2

sudo docker compose up -d

Check whether it started:

sudo docker ps -a --filter name=mikrotik

View the startup logs:

sudo docker logs --tail 100 mikrotik

Follow the logs live:

sudo docker logs -f mikrotik
4. Access MikroTik

Replace YOUR_SERVER_IP with your VPS public IP address.

Service

	

Address




Winbox

	

YOUR_SERVER_IP:8291




SSH

	

YOUR_SERVER_IP:2222




HTTP

	

http://YOUR_SERVER_IP:8080




HTTPS

	

https://YOUR_SERVER_IP:8443




MikroTik API

	

YOUR_SERVER_IP:8728




API-SSL

	

YOUR_SERVER_IP:8729




WireGuard

	

YOUR_SERVER_IP:13231/UDP

These are the configured endpoints; they will only work if the container starts correctly, the relevant RouterOS services are enabled, and networking is supported by the image.

5. Check the container and ports
sudo docker inspect mikrotik --format '{{.State.Status}}'
sudo docker port mikrotik
sudo ss -lntup

If you use UFW, configure firewall rules for the ports you actually need. For example, to allow Winbox only from your own public IP:

sudo ufw allow from YOUR_PUBLIC_IP to any port 8291 proto tcp

Security: Avoid exposing Telnet, FTP, SSH, or the MikroTik API to the entire internet. Restrict access to trusted IP addresses and configure strong RouterOS credentials.

One more detail: your original example uses tag 7.6, while the pull command specifies 7.16.2. The Compose file above uses 7.16.2 for consistency. If that tag is unavailable, check the image's Docker Hub tags and select a published version
