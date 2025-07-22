# 🐳 Deploying eShopOnWeb with Docker and NGINX

This guide explains how to deploy the [eShopOnWeb](https://github.com/dotnet-architecture/eShopOnWeb) application using Docker and NGINX on a Linux server.

---

## ✅ Prerequisites

Make sure your server has the following installed:

- Docker  
- Docker Compose  
- NGINX  
- Git  
- UFW (firewall)

---

## 📦 1. Clone the Repository

```bash
git clone https://github.com/dotnet-architecture/eShopOnWeb.git
cd eShopOnWeb
🐋 2. Start the App with Docker Compose

The project already includes Dockerfile and docker-compose.yml.

sudo docker compose up -d

Check containers:

docker ps
3. Open Firewall Ports

Allow HTTP traffic:

sudo ufw allow 80
sudo ufw enable
sudo ufw status

🌐 4. Configure NGINX Reverse Proxy
📥 Install NGINX

sudo apt update
sudo apt install nginx -y

Create NGINX Config

sudo nano /etc/nginx/sites-available/eshop

Paste this:

server {
    listen 80;

    server_name your_server_ip_or_domain;

    location / {
        proxy_pass http://localhost:5106;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection keep-alive;
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}

Replace your_server_ip_or_domain with your actual IP or domain.
🔗 Enable and Restart NGINX

sudo ln -s /etc/nginx/sites-available/eshop /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx

🚀 5. Access the App

Open your browser and visit:

http://your_server_ip

🛠 6. Manage Containers

    List containers:

docker ps

    View logs:

docker logs eshoponweb-eshopwebmvc-1

    Stop the stack:

docker compose down
