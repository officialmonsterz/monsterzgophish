![gophish logo](https://raw.github.com/gophish/gophish/master/static/images/gophish_purple.png)

Gophish
=======

![Build Status](https://github.com/gophish/gophish/workflows/CI/badge.svg) [![GoDoc](https://godoc.org/github.com/gophish/gophish?status.svg)](https://godoc.org/github.com/gophish/gophish)

Gophish: Open-Source Phishing Toolkit

[Gophish](https://getgophish.com) is an open-source phishing toolkit designed for businesses and penetration testers. It provides the ability to quickly and easily setup and execute phishing engagements and security awareness training.

# MonsterzGoPhish  (t.me/officialmonsterz)

**🎣 MonsterzGoPhish** — A stealth-optimized fork of the popular open-source phishing simulation toolkit **Gophish**.

---

## Why MonsterzGoPhish is Better than Original Gophish

While the original Gophish is excellent, it has become very well-known in the cybersecurity community. Security tools, blue teams, and researchers can easily fingerprint and block Gophish instances. **MonsterzGoPhish** fixes this by focusing heavily on **Operational Security (OPSEC)**.

### Key Advantages & Improvements

| Feature                          | Original Gophish                          | MonsterzGoPhish                              | Benefit |
|----------------------------------|-------------------------------------------|----------------------------------------------|-------|
| **Version Fingerprint**          | Shows `0.12.1`                            | Changed to `2.0.0`                           | Hides real version |
| **Server Header**                | `X-Server: gophish`                       | Removed + Fake `Apache/2.4.41` header        | Looks like a normal web server |
| **Transparency Feature**         | `+` on tracking URL reveals Gophish       | Disabled (returns 404)                       | Prevents easy detection |
| **Admin Server Port**            | Usually `127.0.0.1:3333`                  | `0.0.0.0:3333` (ready for proxy)             | Easier production setup |
| **Phishing Server Port**         | Port 80 (needs root)                      | Port `8080` (no root required)               | More convenient & secure |
| **robots.txt**                   | Disallows all crawling                    | Allows crawling (`Allow: /`)                 | Looks more legitimate |
| **Security Headers**             | Strict `X-Frame-Options: DENY`            | Removed (allows iframe embedding)            | More flexible phishing pages |
| **Internal Server Name**         | `"gophish"`                               | Changed to `"IGNORE"`                        | Extra layer of obfuscation |
| **Default Config**               | More restrictive                          | Production-friendly defaults                 | Faster deployment |

### Why Everyone Should Use MonsterzGoPhish

- **Stronger OPSEC** — Much harder for antivirus, EDR, and security researchers to detect.
- **Lower Chance of Being Blocked** — Phishing pages look like normal Apache websites.
- **Easier Production Setup** — Runs smoothly behind Nginx with proper SSL.
- **Same Great Features** — All original Gophish capabilities are preserved (campaigns, tracking, reporting, templates, etc.).
- **Regularly Maintainable** — Easy to merge updates from upstream Gophish when needed.
- **Community & Ease** — Same familiar interface, but with modern stealth improvements.

**In short:**  
If you are doing **security awareness training**, **red teaming**, or **authorized penetration testing**, **MonsterzGoPhish** gives you the same power as Gophish but with significantly better stealth and lower detection risk.

---

## Quick Start

### Prerequisites
- Ubuntu/Debian VPS (recommended)
- Domain name (e.g., yourdomain.com)
- Basic terminal knowledge

### Installation (Full Production Setup)

See the detailed setup guide in the repository or follow the step-by-step instructions you already used.

### Login
- URL: `https://admin.yourdomain.com`
- Default Username: `admin`
- Password: Shown in logs on first run (`journalctl -u monsterzgophish`)

---

## Keeping It Updated

```bash
cd /opt/monsterzgophish
git pull
go build
sudo systemctl restart monsterzgophish



OFFICAL MONSTERGOPHISH SETUP


Step 1: Update System & Install Packages

sudo apt update && sudo apt upgrade -y

sudo apt install curl wget git unzip nano certbot python3-certbot-nginx nginx golang-go ufw -y


Step 2: Clone & Build MonsterzGoPhish

cd /opt
sudo git clone https://github.com/officialmonsterz/monsterzgophish.git
cd monsterzgophish
go mod tidy
go build

ls

MAKE SURE YOU SEE gophish in there

Step 3: Firewall

sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw --force enable
sudo ufw reload
sudo ufw status



Step 4: DNS (Must be done in your domain registrar)Create these A records pointing to your VPS IP:@ → your VPS IP
admin → your VPS IP

Wait 10–30 minutes, then test:


ping yourdomain.com
ping admin.yourdomain.com


Step 5: Get SSL Certificates


sudo fuser -k 80/tcp
sudo fuser -k 443/tcp

sudo certbot certonly --standalone -d yourdomain.com -d admin.yourdomain.com

IF IT DIDNT WORK, TRY AGAIN, AND IF IT DIDNT, JUST USE THE NEXT COMMAND


sudo certbot --nginx -d yourdomain.com -d admin.yourdomain.com


Step 6: Configure Nginx

Phishing Site Config:

sudo nano /etc/nginx/sites-available/phishing




Paste this:



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



Admin Panel Config:

sudo nano /etc/nginx/sites-available/admin


Paste this:



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



Enable Nginx configs:

sudo ln -s /etc/nginx/sites-available/phishing /etc/nginx/sites-enabled/
sudo ln -s /etc/nginx/sites-available/admin /etc/nginx/sites-enabled/
sudo rm -f /etc/nginx/sites-enabled/default

sudo nginx -t && sudo systemctl restart nginx



Step 7: Create Systemd Service

sudo nano /etc/systemd/system/monsterzgophish.service

Paste this:


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


Step 8: Configure Gophish (Most Important)

cd /opt/monsterzgophish
sudo nano config.json


Replace everything with this:


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


Step 9: Final Start

sudo systemctl daemon-reload
sudo systemctl enable monsterzgophish

# Fresh start
sudo systemctl stop monsterzgophish
sudo rm -f gophish.db
sudo systemctl start monsterzgophish

sudo systemctl status monsterzgophish


Step 10: Get Admin Password

sudo journalctl -u monsterzgophish -n 100 | grep -i password


Final TestOpen browser → Go to https://admin.yourdomain.com
Login with:Username: admin
Password: (the one from the command above)

You should now enter the dashboard with green padlock.Useful Commands (Save These)Restart Gophish: sudo systemctl restart monsterzgophish
See logs: sudo journalctl -u monsterzgophish -f
See Nginx status: sudo systemctl status nginx
Check certificates: sudo certbot certificates

You are now fully set up.


t.me/officialmonsterz
shapads@tutamail.com
