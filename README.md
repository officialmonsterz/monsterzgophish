<p align="center">
  <img src="https://raw.github.com/gophish/gophish/master/static/images/gophish_purple.png" alt="MonsterzGoPhish Logo" width="350">
</p>

<h1 align="center">🎣 MonsterzGoPhish</h1>

<p align="center">
  <b>The Ultimate Stealth-Optimized Phishing Simulation Toolkit</b><br>
  <i>Forked from Gophish — Built for Red Teams, Penetration Testers & Security Awareness Professionals</i>
</p>

<p align="center">
  <a href="https://github.com/officialmonsterz/monsterzgophish">
    <img src="https://img.shields.io/github/stars/officialmonsterz/monsterzgophish?style=for-the-badge&logo=github&color=gold" alt="Stars">
  </a>
  <a href="https://github.com/officialmonsterz/monsterzgophish/actions">
    <img src="https://img.shields.io/github/actions/workflow/status/officialmonsterz/monsterzgophish/ci.yml?style=for-the-badge&logo=github-actions&color=green" alt="CI Build">
  </a>
  <a href="https://t.me/officialmonsterz">
    <img src="https://img.shields.io/badge/Telegram-Contact-blue?style=for-the-badge&logo=telegram" alt="Telegram">
  </a>
  <img src="https://img.shields.io/badge/Version-2.0.0-brightgreen?style=for-the-badge" alt="Version">
  <img src="https://img.shields.io/badge/Go-1.2x-00ADD8?style=for-the-badge&logo=go" alt="Go">
  <img src="https://img.shields.io/badge/License-MIT-blue?style=for-the-badge" alt="License">
  <img src="https://img.shields.io/badge/Status-Active-success?style=for-the-badge" alt="Status">
</p>

<p align="center">
  <b>
    <a href="#-why-monsterzgophish-is-better-than-original-gophish">Why MonsterzGoPhish?</a> •
    <a href="#-quick-start">Quick Start</a> •
    <a href="#-installation-full-production-setup">Installation</a> •
    <a href="#-keeping-it-updated">Updates</a> •
    <a href="#-credits">Credits</a>
  </b>
</p>

---

<br>

## 📖 About MonsterzGoPhish

> **MonsterzGoPhish** is a **stealth-optimized fork** of the popular open-source phishing simulation toolkit **[Gophish](https://getgophish.com)**.  
> It provides the ability to quickly and easily setup and execute **phishing engagements** and **security awareness training** — but with significantly **stronger OPSEC** to evade detection.

<br>

---

## 🥇 Why MonsterzGoPhish is Better than Original Gophish

While the original Gophish is excellent, it has become **very well-known** in the cybersecurity community. Security tools, blue teams, EDRs, and researchers can easily **fingerprint and block** standard Gophish instances.

**MonsterzGoPhish fixes this** by focusing heavily on **Operational Security (OPSEC)** — making your instances blend in as normal, legitimate web servers.

<br>

### 📊 Feature Comparison

| # | Feature | 🔴 Original Gophish | 🟢 MonsterzGoPhish | 🎯 Benefit |
|:--:|:--------|:---------------------:|:--------------------:|:------------|
| 1 | **Version Fingerprint** | Shows `0.12.1` | Changed to `2.0.0` 🛡️ | Hides real version from scanners |
| 2 | **Server Header** | `X-Server: gophish` | Removed + Fake `Apache/2.4.41` header 🕵️ | Looks like a normal Apache web server |
| 3 | **Transparency Feature** | `+` on tracking URL reveals Gophish | **Disabled** (returns 404) 🚫 | Prevents easy detection by blue teams |
| 4 | **Admin Server Port** | Usually `127.0.0.1:3333` | `0.0.0.0:3333` (ready for proxy) 🔄 | Easier production setup behind Nginx |
| 5 | **Phishing Server Port** | Port 80 (needs root) | Port `8080` (no root required) 🙅 | More convenient & secure |
| 6 | **robots.txt** | Disallows all crawling | Allows crawling (`Allow: /`) 🕸️ | Looks more legitimate to bots |
| 7 | **Security Headers** | Strict `X-Frame-Options: DENY` | **Removed** (allows iframe embedding) 🔓 | More flexible phishing pages |
| 8 | **Internal Server Name** | `"gophish"` | Changed to `"IGNORE"` 🤫 | Extra layer of obfuscation |
| 9 | **Default Config** | More restrictive | **Production-friendly defaults** ⚙️ | Faster deployment, less tinkering |
| 10 | **SSL Setup** | Manual, often problematic | **Nginx handles SSL seamlessly** 🔐 | No certificate headaches |

<br>

### 🏆 Why Everyone Should Use MonsterzGoPhish

| ✅ | Advantage | Why It Matters |
|:--:|:-----------|:---------------|
| 🛡️ | **Stronger OPSEC** | Much harder for antivirus, EDR, and security researchers to detect your instance |
| 🚫 | **Lower Chance of Being Blocked** | Phishing pages look like normal Apache websites — blend in with thousands of other sites |
| ⚡ | **Easier Production Setup** | Runs smoothly behind Nginx with proper SSL — no complex configuration |
| 🔄 | **Same Great Features** | All original Gophish capabilities preserved (campaigns, tracking, reporting, templates) |
| 📦 | **Regularly Maintainable** | Easy to merge updates from upstream Gophish when needed |
| 👥 | **Community & Familiarity** | Same interface everyone knows, but with modern stealth improvements |
| 🔐 | **SSL Handled Automatically** | No certificate struggles — Nginx + Certbot handle everything for you |

> **In Short:** If you are doing **security awareness training**, **red teaming**, or **authorized penetration testing**, **MonsterzGoPhish** gives you the same power as Gophish but with **significantly better stealth** and **much lower detection risk**.

<br>

---

## ⚡ Quick Start

### 📋 Prerequisites

| Requirement | Details |
|:------------|:--------|
| 💻 **VPS** | Ubuntu/Debian (recommended) — any cloud provider works |
| 🌐 **Domain** | A domain name (e.g., `yourdomain.com`) with DNS access |
| 🧠 **Knowledge** | Basic terminal/command line familiarity |
| ✅ **Authorization** | **You must have explicit permission** to perform phishing tests |

<br>

### 🚀 Deployment at a Glance

┌──────────────────────────────────────────────────────────────┐ │ MonsterzGoPhish Setup │ ├──────────────────────────────────────────────────────────────┤ │ │ │ 1️⃣ Update System & Install Packages │ │ 2️⃣ Clone & Build MonsterzGoPhish │ │ 3️⃣ Configure Firewall (UFW) │ │ 4️⃣ Set Up DNS Records │ │ 5️⃣ Obtain SSL Certificates (Certbot) │ │ 6️⃣ Configure Nginx (Reverse Proxy) │ │ 7️⃣ Create Systemd Service (Auto-start) │ │ 8️⃣ Configure Gophish (config.json) │ │ 9️⃣ Start & Enable Service │ │ 🔟 Retrieve Admin Password & Login │ │ │ └──────────────────────────────────────────────────────────────┘

<br>

---

## 📦 Installation (Full Production Setup)

> **⏱️ Estimated Time:** 15–20 minutes  
> **📌 Replace `yourdomain.com` with your actual domain throughout this guide.**

<br>

### 🔧 Step 1: Update System & Install Packages

```bash
sudo apt update && sudo apt upgrade -y

sudo apt install curl wget git unzip nano certbot python3-certbot-nginx nginx golang-go ufw -y

Package	Purpose
curl / wget	Web requests & downloads
git	Clone the repository
unzip	Extract archives
nano	Text editor
certbot	SSL certificate automation
python3-certbot-nginx	Certbot Nginx plugin
nginx	Reverse proxy & SSL termination
golang-go	Compile Go source code
ufw	Firewall management

📥 Step 2: Clone & Build MonsterzGoPhish




cd /opt
sudo git clone https://github.com/officialmonsterz/monsterzgophish.git
cd monsterzgophish
go mod tidy
go build
✅ Verify Build




ls
⚠️ MAKE SURE YOU SEE gophish in there



File	Purpose
gophish	Compiled binary (the main executable)
config.json	Configuration file
gophish.db	SQLite database (auto-created on first run)
static/	Web assets (CSS, JS, images)
templates/	Email & landing page templates

🔥 Step 3: Firewall Configuration




sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw --force enable
sudo ufw reload
sudo ufw status


Port	Service	Purpose
22/tcp	SSH	Secure remote access to your VPS
80/tcp	HTTP	Let's Encrypt certificate verification
443/tcp	HTTPS	Production traffic (phishing & admin)

🌐 Step 4: DNS Setup
Must be done in your domain registrar's DNS management panel

📝 Create These A Records


Record	Type	Value
@	A	your_vps_ip_address
admin	A	your_vps_ip_address
⏳ Wait & Verify
DNS propagation takes 10–30 minutes. Test with:





ping yourdomain.com
ping admin.yourdomain.com

🔐 Step 5: Get SSL Certificates




sudo fuser -k 80/tcp
sudo fuser -k 443/tcp




sudo certbot certonly --standalone -d yourdomain.com -d admin.yourdomain.com
⚠️ IF IT DIDN'T WORK, TRY AGAIN, AND IF IT DIDN'T, JUST USE THE NEXT COMMAND





sudo certbot --nginx -d yourdomain.com -d admin.yourdomain.com

⚙️ Step 6: Configure Nginx
Nginx acts as a reverse proxy — it handles SSL/TLS termination and forwards traffic to Gophish running locally.


📄 Phishing Site Config




sudo nano /etc/nginx/sites-available/phishing
Paste this:

nginx



server {
    listen 80;
    server_name yourdomain.com www.yourdomain.com;
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    server_name yourdomain.com www.yourdomain.com;

    ssl_certificate /etc/letsencrypt/live/yourdomain.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/yourdomain.com/privkey.pem;

    location / {
        proxy_pass http://127.0.0.1:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}

📄 Admin Panel Config




sudo nano /etc/nginx/sites-available/admin
Paste this:

nginx



server {
    listen 80;
    server_name admin.yourdomain.com;
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    server_name admin.yourdomain.com;

    ssl_certificate /etc/letsencrypt/live/yourdomain.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/yourdomain.com/privkey.pem;

    location / {
        proxy_pass http://127.0.0.1:3333;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto "";
        proxy_set_header Referer "";
    }
}

🔗 Enable Nginx Configs




sudo ln -s /etc/nginx/sites-available/phishing /etc/nginx/sites-enabled/
sudo ln -s /etc/nginx/sites-available/admin /etc/nginx/sites-enabled/
sudo rm -f /etc/nginx/sites-enabled/default




sudo nginx -t && sudo systemctl restart nginx

🧩 Step 7: Create Systemd Service
This ensures MonsterzGoPhish starts automatically on boot and restarts if it crashes.





sudo nano /etc/systemd/system/monsterzgophish.service
Paste this:

ini



[Unit]
Description=Monsterz GoPhish
After=network.target

[Service]
Type=simple
User=root
WorkingDirectory=/opt/monsterzgophish
ExecStart=/opt/monsterzgophish/gophish
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target

🔧 Step 8: Configure Gophish (Most Important)




cd /opt/monsterzgophish
sudo nano config.json
Replace everything with this:

json



{
  "admin_server": {
    "listen_url": "0.0.0.0:3333",
    "use_tls": false,
    "cert_path": "",
    "key_path": "",
    "trusted_origins": ["https://admin.yourdomain.com"]
  },
  "phish_server": {
    "listen_url": "0.0.0.0:8080",
    "use_tls": false,
    "cert_path": "",
    "key_path": ""
  },
  "db_name": "sqlite3",
  "db_path": "gophish.db",
  "migrations_prefix": "db/db_",
  "contact_address": "",
  "logging": {
    "filename": "/var/log/gophish.log",
    "level": "info"
  }
}
🔑 Config Breakdown


Key	Value	Why?
admin_server.listen_url	0.0.0.0:3333	Listen on all interfaces (Nginx proxying)
admin_server.use_tls	false	TLS handled by Nginx — no double-encryption
admin_server.trusted_origins	["https://admin.yourdomain.com"]	CORS whitelist for admin panel
phish_server.listen_url	0.0.0.0:8080	Non-root port — more secure & convenient
phish_server.use_tls	false	TLS handled by Nginx
db_name	sqlite3	Simple, portable database

🚀 Step 9: Final Start




sudo systemctl daemon-reload
sudo systemctl enable monsterzgophish
🧹 Fresh Start (clears previous DB)




sudo systemctl stop monsterzgophish
sudo rm -f gophish.db
sudo systemctl start monsterzgophish
📊 Check Status




sudo systemctl status monsterzgophish

🔑 Step 10: Get Admin Password




sudo journalctl -u monsterzgophish -n 100 | grep -i password
📝 Look for the line containing your temporary admin password — it appears only on first run.


✅ Final Test


#	Action	Expected Result
1	Open browser → https://admin.yourdomain.com	Admin login page loads
2	Enter Username: admin	Field accepts input
3	Enter Password: (from Step 10)	Successful login
4	🔒 Check browser address bar	Green padlock (valid SSL)
5	Dashboard loads	Full Gophish dashboard with all features

📐 Architecture Overview



                              ┌──────────────────────┐
                              │      INTERNET        │
                              │   🌐 Global Network  │
                              └─────┬──────────┬─────┘
                                    │          │
                          ┌─────────┘          └─────────┐
                          ▼                              ▼
              ┌─────────────────────┐      ┌─────────────────────┐
              │   yourdomain.com    │      │  admin.yourdomain   │
              │    Port 80/443      │      │   .ca Port 80/443   │
              │   (Phishing Pages)  │      │   (Admin Dashboard) │
              └──────────┬──────────┘      └──────────┬──────────┘
                         │                             │
                         ▼                             ▼
              ┌─────────────────────┐      ┌─────────────────────┐
              │   🟢 Nginx (SSL)   │      │   🟢 Nginx (SSL)   │
              │  Reverse Proxy     │      │  Reverse Proxy     │
              │  Certificate Term. │      │  Certificate Term. │
              └──────────┬──────────┘      └──────────┬──────────┘
                         │                             │
                         ▼                             ▼
              ┌─────────────────────┐      ┌─────────────────────┐
              │  🎯 Phish Server   │      │  ⚙️  Admin Server   │
              │  :8080 (HTTP)      │      │  :3333 (HTTP)       │
              │  (Serves phishing  │      │  (Dashboard &       │
              │   landing pages)   │      │   campaign mgmt)    │
              └──────────┬──────────┘      └──────────┬──────────┘
                         │                             │
                         └──────────┬──────────────────┘
                                    ▼
                         ┌─────────────────────┐
                         │   📦 Gophish Core  │
                         │   (Go Binary)      │
                         │   SQLite3 DB       │
                         │   Templates        │
                         │   Campaigns        │
                         └─────────────────────┘

📝 Useful Commands (Save These)


Command	Description
sudo systemctl restart monsterzgophish	🔄 Restart MonsterzGoPhish
sudo systemctl stop monsterzgophish	⏹️ Stop MonsterzGoPhish
sudo systemctl start monsterzgophish	▶️ Start MonsterzGoPhish
sudo journalctl -u monsterzgophish -f	📋 Follow live logs
sudo journalctl -u monsterzgophish -n 200	📄 View last 200 log lines
sudo systemctl status monsterzgophish	📊 Check service status
sudo systemctl status nginx	🌐 Check Nginx status
sudo certbot certificates	🔐 View SSL certificate info
sudo certbot renew	🔄 Auto-renew certificates
sudo nginx -t	✅ Test Nginx configuration
sudo systemctl restart nginx	🔄 Restart Nginx
sudo ufw status verbose	🔥 View firewall rules
sudo rm -f /opt/monsterzgophish/gophish.db	🗑️ Reset database (fresh start)

🔄 Keeping It Updated




cd /opt/monsterzgophish
git pull
go build
sudo systemctl restart monsterzgophish
That's it! Your instance will be updated with the latest features and fixes.


🐞 Troubleshooting
❌ Certificate Issues




# Check certificate status
sudo certbot certificates

# Renew if needed
sudo certbot renew

# Verify Nginx config
sudo nginx -t
❌ Gophish Won't Start




# Check logs
sudo journalctl -u monsterzgophish -n 50

# Verify config.json syntax
cat /opt/monsterzgophish/config.json

# Ensure no port conflicts
sudo netstat -tulpn | grep -E '3333|8080'
❌ 502 Bad Gateway (Nginx)
Ensure Gophish is running: sudo systemctl status monsterzgophish
Check Nginx proxy targets in site configs
Verify config.json has correct listen_url values

📜 License
MonsterzGoPhish is released under the MIT License.
Original Gophish is © 2013-2021 Jordan Wright — getgophish.com


🌟 Credits
MonsterzGoPhish

MonsterzGoPhish
Stealth-optimized phishing simulation toolkit for authorized security professionals

Telegram GitHub Email

🔗 Official Repository:
https://github.com/officialmonsterz/monsterzgophish.git


❤️ Built with passion for the cybersecurity community
Use responsibly. Always have explicit authorization.

Star Fork

Footer Wave

❌ 502 Bad Gateway (Nginx)
Ensure Gophish is running: sudo systemctl status monsterzgophish
Check Nginx proxy targets in site configs
Verify config.json has correct listen_url values
📜 License
MonsterzGoPhish is released under the MIT License.
Original Gophish is © 2013-2021 Jordan Wright — getgophish.com


🌐 OFFICIAL MONSTERZGOPHISH

MonsterzGoPhish


👑 Created & Maintained By





🔗 Official Repository:
https://github.com/officialmonsterz/monsterzgophish.git


🌍 Follow MonsterzGoPhish:

📱 Telegram: t.me/officialmonsterz
🐙 GitHub: github.com/officialmonsterz
📧 Email: shapads@tutamail.com



❤️ Built with passion for the cybersecurity community by @officialmonsterz
Use responsibly. Always have explicit authorization.


Telegram    Star    Email


Footer Wave


OFFICIAL MONSTERZGOPHISH
t.me/officialmonsterz | github.com/officialmonsterz | shapads@tutamail.com


You are now fully set up. Happy Hacking! 🎯
