# 🚀 OFFICIAL MONSTERZ GOPHISH SETUP

**Complete Professional Deployment Guide**

![GoPhish](https://img.shields.io/badge/GoPhish-Monsterz%20Edition-red?style=for-the-badge)

> **Maintained & Enhanced by Official Monsterz**

---

### 📋 Credits & Support
- **GitHub**: [github.com/officialmonsterz](https://github.com/officialmonsterz/monsterzgophish.git)
- **Telegram**: [t.me/officialmonsterz](https://t.me/officialmonsterz)
- **Contact**: shapads@tutamail.com

---

📋 Table of Contents

#	Step	Description
1	Update System & Install Packages	System prep & dependencies
2	Clone & Build MonsterzGoPhish	Download & compile
3	Firewall Configuration	Lock down access
4	DNS Setup	Point your domain
5	SSL Certificates	Enable HTTPS
6	Nginx Configuration	Reverse proxy setup
7	Systemd Service	Auto-start on boot
8	Gophish Configuration	Core setup
9	Final Start	Launch it all
10	Get Admin Password	First login
11	Final Test	Verify everything
12	Useful Commands	Quick reference

## Step 1: Update System & Install Packages

```bash
sudo apt update && sudo apt upgrade -y

sudo apt install curl wget git unzip nano certbot python3-certbot-nginx nginx golang-go ufw -y

Package	Purpose
curl, wget	Download utilities
git	Source control
unzip	Archive extraction
nano	Text editor
certbot	SSL certificate automation
nginx	Reverse proxy server
golang-go	Go compiler
ufw	Firewall management
