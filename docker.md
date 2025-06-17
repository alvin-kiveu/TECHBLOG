# 🚢 Docker Installation Guide

This guide will help you install and set up Docker on your system so you can start building, shipping, and running containers.

---

## 📌 Requirements

- A 64-bit version of Ubuntu (20.04 or later recommended)
- A user account with `sudo` privileges
- Internet access

---

## ⚙️ Installation Steps (Ubuntu)

### 1. Uninstall Old Versions

```bash
sudo apt-get remove docker docker-engine docker.io containerd runc
```
2. Update Package Index and Install Dependencies

```bash
sudo apt-get update
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```

3. Add Docker’s Official GPG Key

```bash
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
    sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```
4. Set Up the Docker Repository
```bash
echo \
  "deb [arch=$(dpkg --print-architecture) \
  signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
5. Install Docker Engine
```bash
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

✅ Verify Installation
```bash
sudo docker --version
sudo docker run hello-world
```

You should see a message saying "Hello from Docker!"

🔐 Optional: Run Docker Without sudo

```bash
sudo usermod -aG docker $USER
newgrp docker
```

Then verify with:

bash
Copy
Edit
docker run hello-world
📦 Docker Compose (Optional)
If you need Docker Compose:

```bash
sudo apt-get install docker-compose
docker-compose --version
```

📚 Useful Docker Commands
docker ps – List running containers

docker images – List downloaded images

docker run -it ubuntu bash – Run an Ubuntu container interactively

docker build -t my-image . – Build an image from a Dockerfile

docker-compose up – Start services using Compose

🧼 Uninstall Docker
```bash
sudo apt-get purge docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
```

📝 License
This guide is open-source and free to use.

💡 Tip: For detailed documentation, visit the official Docker docs


Let me know if you want this customized for **Windows**, **macOS**, or Docker installation with **scripts** or **Ansible**.

