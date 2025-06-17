To install and access UISP (formerly UNMS) for the first time, follow these steps based on the official guide:

Prerequisites
Server Requirements:

OS: Ubuntu 18.04/20.04/22.04 (recommended) or Debian 10/11.

Resources: Minimum 2 CPU cores, 4GB RAM, 10GB disk space.

Static IP address for the server.

Ports 80 (HTTP), 443 (HTTPS), and 10001 (device communication) open in the firewall.

Installation Steps
1. Prepare the System
Update packages:

bash
sudo apt update && sudo apt upgrade -y
Install dependencies:

bash
sudo apt install -y curl
2. Run the Installation Script
Download and execute the UISP installer:

bash
curl -fsSL https://uisp.ui.com/v1/install > install.sh
sudo bash install.sh
Follow prompts to:

Accept the license agreement.

Configure network settings (use default settings if unsure).

Set a hostname (e.g., uisp.yourdomain.com).

3. Configure Firewall
Allow essential ports:

bash
sudo ufw allow 80/tcp    # HTTP
sudo ufw allow 443/tcp   # HTTPS
sudo ufw allow 10001/udp # Device discovery
sudo ufw enable
Accessing UISP
Open the Web Interface:

In a browser, navigate to:

https://<SERVER_IP> (replace with your server’s IP/hostname).

Ignore SSL warnings during initial setup (self-signed certificate).

First-Time Setup Wizard:

Create an admin account (email/password).

Configure basic settings (organization name, time zone).

Add a Site (e.g., "Main Office") to organize devices.

Connect Devices:

For Ubiquiti devices (UniFi, EdgeMax, etc.):

Ensure devices are on the same network as UISP.

Go to Devices > Discover. UISP will auto-detect supported devices.

Click Adopt to add devices to UISP.

Post-Installation
Backup: Enable automatic backups via Settings > Backup.

Updates: UISP auto-updates by default (manage in Settings > Updates).

Remote Access: Use UISP Connect for secure remote access.

Troubleshooting
Installation Fails?:

Verify OS compatibility and dependencies.

Check logs: journalctl -u docker (UISP runs in Docker containers).

Devices Not Found?:

Confirm port 10001/udp is open.

Ensure devices and UISP are on the same Layer-2 network.

For details, see the full guide:
