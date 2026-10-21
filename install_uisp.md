UISP (formerly UNMS) – First-Time Installation and Access Guide

===============================================================
1. PREREQUISITES
===============================================================

Server Requirements:
- OS: Ubuntu 18.04 / 20.04 / 22.04 (recommended) or Debian 10 / 11
- Resources: Minimum 2 CPU cores, 4GB RAM, 10GB disk space
- Static IP address for the server
- Ports 80 (HTTP), 443 (HTTPS), and 10001 (device communication) open in the firewall

===============================================================
2. INSTALLATION STEPS
===============================================================

2.1 Prepare the System

Update packages:
    sudo apt update && sudo apt upgrade -y

Install dependencies:
    sudo apt install -y curl

2.2 Run the Installation Script

Download and execute the UISP installer:
    curl -fsSL https://uisp.ui.com/v1/install > install.sh
    sudo bash install.sh

Follow the prompts to:
- Accept the license agreement.
- Configure network settings (use default settings if unsure).
- Set a hostname (e.g., uisp.yourdomain.com).

2.3 Configure Firewall

Allow essential ports:
    sudo ufw allow 80/tcp     # HTTP
    sudo ufw allow 443/tcp    # HTTPS
    sudo ufw allow 10001/udp  # Device discovery
    sudo ufw enable

===============================================================
3. ACCESSING UISP
===============================================================

3.1 Open the Web Interface

In a browser, navigate to:
    https://<SERVER_IP>

Replace <SERVER_IP> with your server's IP address or hostname.

Note: Ignore SSL warnings during initial setup (self-signed certificate).

3.2 First-Time Setup Wizard

- Create an admin account (email/password).
- Configure basic settings (organization name, time zone).
- Add a Site (e.g., "Main Office") to organize devices.

3.3 Connect Devices

For Ubiquiti devices (UniFi, EdgeMax, etc.):
- Ensure devices are on the same network as UISP.
- Go to Devices > Discover. UISP will auto-detect supported devices.
- Click Adopt to add devices to UISP.

===============================================================
4. POST-INSTALLATION
===============================================================

Backup:
- Enable automatic backups via Settings > Backup.

Updates:
- UISP auto-updates by default (manage in Settings > Updates).

Remote Access:
- Use UISP Connect for secure remote access.

===============================================================
5. TROUBLESHOOTING
===============================================================

Installation Fails?
- Verify OS compatibility and dependencies.
- Check logs:
    journalctl -u docker

  Note: UISP runs in Docker containers.

Devices Not Found?
- Confirm port 10001/udp is open.
- Ensure devices and UISP are on the same Layer-2 network.

===============================================================
6. FULL GUIDE
===============================================================

For details, see the full guide:
[Add full guide URL here]
