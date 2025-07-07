# VPS Management Master Commands

A curated collection of essential commands and configurations to manage an Ubuntu 22.04 VPS running Node.js applications behind NGINX with SSL, PM2 process management, and basic security hardening.

---

## 🔖 Table of Contents

1. Prerequisites
2. System Basics
3. Node.js & NPM
4. Git & Deployment
5. Environment Variables
6. Project Dependencies
7. Process Management with PM2
8. NGINX Reverse Proxy
9. SSL with Certbot
10. Firewall (UFW)
11. Monitoring & Logs
12. Reboot & Maintenance
13. Security Enhancements

## 🔌 Prerequisites

- **Ubuntu 22.04 LTS** on your VPS  
- **Root** or **sudo** user access  
- **Domain name** pointed (A record) to your VPS IP  
- Basic familiarity with the Linux shell  


## 🖥️ System Basics

```bash
# 1. Update package index
sudo apt update

# 2. Upgrade installed packages
sudo apt upgrade -y

# 3. Full upgrade (production caution)
sudo apt full-upgrade -y

# 4. Clean up unused packages
sudo apt autoremove -y
sudo apt autoclean

# tips: Set your server timezone to UTC or your local zone:
sudo timedatectl set-timezone UTC
```

## 📦 Node.js & NPM

```bash
# Install the official LTS release:
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs

# Verify:
node -v
npm -v
```

## 🔗 Git & Deployment

```bash
# Install Git
sudo apt install -y git

# Clone your repository
git clone https://github.com/<YOUR_USERNAME>/<YOUR_REPO>.git ~/apps/<YOUR_APP>
cd ~/apps/<YOUR_APP>
```

## 🔑 Environment Variables

```bash
cd ~/apps/<YOUR_APP>
nano .env

# .env example
PORT=
NODE_ENV=production
DB_HOST=localhost
DB_USER=appuser
DB_PASS=strongpassword
```



## 📥 Project Dependencies

```bash
cd ~/apps/<YOUR_APP>
npm install
or 
yarn install
```

## 🚀 Process Management with PM2

```bash
# Install PM2 globally
sudo npm install -g pm2

# Start app via npm script named "start"
pm start pm2 --name "<APP_NAME>"

# Or directly from your build output
pm2 start dist/index.js --name "<APP_NAME>"

# Generate startup script to respawn PM2 on reboot
pm2 startup systemd
# Follow on-screen instructions to run the generated command

# Save current process list
pm2 save

# View running processes & logs
pm2 list
pm2 monit
```

## 🌐 NGINX Reverse Proxy

### 1. Install & enable NGINX

```bash
sudo apt install -y nginx
sudo systemctl enable nginx
sudo systemctl start nginx
```

### 2. Configure virtual host

```
#create a file
/etc/nginx/sites-available/mytwicket.com
```

```bash
server {
    listen 80;
    server_name yourdomain.com www.yourdomain.com;

    location / {
        proxy_pass ;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```
```bash
# Enable site & test
sudo ln -s /etc/nginx/sites-available/<YOUR_APP> /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```
```
# Finding nginx files
cd /etc/nginx/sites-available
ls
```
## 🔒 SSL with Certbot

```bash
sudo apt install -y certbot python3-certbot-nginx
sudo certbot --nginx -d yourdomain.com -d www.yourdomain.com

# Automatically renew certs via cron (installed by default):
sudo certbot renew --dry-run
```

## 🛡️ Firewall (UFW)

```bash
# Allow OpenSSH before enabling UFW
sudo ufw allow OpenSSH
sudo ufw allow 'Nginx Full'
sudo ufw enable
sudo ufw status verbose
```

## 📊 Monitoring & Logs

```bash
# Disk usage
df -h

# Memory
free -m

# System load
uptime

# Active processes
top

# NGINX error log
sudo tail -f /var/log/nginx/error.log

# Application logs via PM2
pm2 logs <APP_NAME>
```

## 🔄 Reboot & Maintenance

```bash
# Safe reboot
sudo reboot
```

## 🛠️ Security Enhancements


```bash

# Fail2Ban: Prevent SSH brute-forces
sudo apt install -y fail2ban
sudo systemctl enable fail2ban

# Automatic updates (optional):
sudo apt install unattended-upgrades -y
sudo dpkg-reconfigure unattended-upgrades

# Swap file (if low memory):
sudo fallocate -l 1G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab

```

## Completed Deployment ✅

> Congratulations! Your project is fully deployed on production-ready VPS with NGINX reverse proxy, PM2 process manager, and SSL certificate encryption.

<br/>

---

<div align="center">

<i> Happy deploying! Made by [Siratul804](https://github.com/Siratul804) </i>
  
</div>

---
