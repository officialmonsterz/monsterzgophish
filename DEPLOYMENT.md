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

Package	                                        Purpose
curl, wget	                                    Download utilities
git	                                            Source control
unzip	                                          Archive extraction
nano	                                          Text editor
certbot	                                        SSL certificate automation
nginx	                                          Reverse proxy server
golang-go	                                      Go compiler
ufw	                                            Firewall management


📦 Step 2: Clone & Build MonsterzGoPhish

cd /opt
sudo git clone https://github.com/officialmonsterz/monsterzgophish.git
cd monsterzgophish
export CGO_CFLAGS="-g -O2 -Wno-return-local-addr"

🏗️ Build the Binary

go mod tidy
go build

✅ Verify Build

ls
⚠️ IMPORTANT: Make sure you see gophish in the directory listing. If not, the build failed.

📂 /opt/monsterzgophish/
├── 📄 config.json
├── 📄 gophish          ← THIS MUST EXIST
├── 📄 gophish.db
├── 📂 db/
├── 📂 static/
├── 📂 templates/
├── 📂 ...

File	                                    Purpose
gophish	                                  Compiled binary
config.json	                              Configuration file
gophish.db	                              SQLite database (auto-created)
static/	Web                               assets
templates/	                              Email templates


🔥 Step 3: Firewall Configuration

sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw --force enable
sudo ufw reload
sudo ufw status

🔍 Firewall Status Reference

Port	        Service	          Purpose
22	          SSH	              Secure shell access
80	          HTTP	            Certbot verification
443	          HTTPS	            Encrypted traffic

Status: active

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW       Anywhere
80/tcp                     ALLOW       Anywhere
443/tcp                    ALLOW       Anywhere
22/tcp (v6)                ALLOW       Anywhere (v6)
80/tcp (v6)                ALLOW       Anywhere (v6)
443/tcp (v6)               ALLOW       Anywhere (v6)

🌐 Step 4: DNS Setup

    📍 Must be done in your domain registrar's dashboard (e.g., GoDaddy, Namecheap, Cloudflare, etc.)

📝 Create These A Records

Record             Name	                Type	                  Value	TTL
@ (root)	          A	                  your_vps_ip_address	    300-600
admin	              A	                  your_vps_ip_address	    300-600

🎯 Example (Replace with your actual domain):

@     IN  A  203.0.113.10
admin IN  A  203.0.113.10

⏳ Wait & Verify

DNS propagation takes 10–30 minutes. Test with:

ping alphalaef.ca
ping admin.alphalaef.ca

✅ Expected output:

PING alphalaef.ca (203.0.113.10) 56(84) bytes of data.
64 bytes from 203.0.113.10: icmp_seq=1 ttl=64 time=0.042 ms

🔐 Step 5: Get SSL Certificates

Release Ports (if in use)

sudo fuser -k 80/tcp
sudo fuser -k 443/tcp

Method A: Standalone (Recommended)

sudo certbot certonly --standalone -d alphalaef.ca -d admin.alphalaef.ca

Method B: Nginx Fallback

    If Method A fails, try again. If it still doesn't work, use:

sudo certbot --nginx -d alphalaef.ca -d admin.alphalaef.ca


✅ Certificate Locations

📂 /etc/letsencrypt/live/alphalaef.ca/
├── 📄 fullchain.pem     ← SSL certificate + intermediates
├── 📄 privkey.pem       ← Private key (keep secure!)
└── 📄 cert.pem          ← Certificate only

⚙️ Step 6: Configure Nginx

📄 Phishing Site Config

sudo nano /etc/nginx/sites-available/phishing

Paste the following:

server {
    listen 80;
    server_name alphalaef.ca www.alphalaef.ca;
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    server_name alphalaef.ca www.alphalaef.ca;

    ssl_certificate /etc/letsencrypt/live/alphalaef.ca/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/alphalaef.ca/privkey.pem;

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

Paste the following:

server {
    listen 80;
    server_name admin.alphalaef.ca;
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    server_name admin.alphalaef.ca;

    ssl_certificate /etc/letsencrypt/live/alphalaef.ca/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/alphalaef.ca/privkey.pem;

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

✅ Test & Restart

sudo nginx -t && sudo systemctl restart nginx

Expected output:

nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful

🚀 Step 7: Create Systemd Service

sudo nano /etc/systemd/system/monsterzgophish.service

Paste the following:

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

🔧 Service Breakdown

Directive	              Value	                        Description
Type	                  simple	                      Process runs in foreground
User	                  root	                        Run as root
WorkingDirectory	      /opt/monsterzgophish	        Where Gophish lives
ExecStart	              /opt/monsterzgophish/gophish	The binary to run
Restart	                always	                      Auto-restart on crash
RestartSec	            5	Wait                        5 seconds before restart


🎯 Step 8: Configure Gophish (Most Important)

cd /opt/monsterzgophish
sudo nano config.json

✏️ Replace Everything With This:

{
  "admin_server": {
    "listen_url": "0.0.0.0:3333",
    "use_tls": false,
    "cert_path": "",
    "key_path": "",
    "trusted_origins": ["https://admin.alphalaef.ca"]
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


📋 Configuration Explained


Section	                    Setting	                  Value	                                              Why
admin_server	              listen_url	              0.0.0.0:3333	                                      Listen on all interfaces, port 3333
admin_server	              use_tls	                  false	TLS handled by Nginx
admin_server	              trusted_origins	          ["https://admin.alphalaef.ca"]	                    CSRF protection for admin domain
phish_server	              listen_url	              0.0.0.0:8080	                                      Listen on all interfaces, port 8080
phish_server	              use_tls	                  false	                                              TLS handled by Nginx
db_name		                                            sqlite3	                                            Lightweight local database
logging	                    filename	                /var/log/gophish.log	                              Centralized logging
logging	                    level	                    info	                                              Standard verbosity


🏁 Step 9: Final Start

sudo systemctl daemon-reload
sudo systemctl enable monsterzgophish

🧹 Fresh Start (Clean Database)

sudo systemctl stop monsterzgophish
sudo rm -f gophish.db
sudo systemctl start monsterzgophish


✅ Check Status

sudo systemctl status monsterzgophish

Expected output:

● monsterzgophish.service - Monsterz GoPhish
     Loaded: loaded (/etc/systemd/system/monsterzgophish.service; enabled; preset: enabled)
     Active: active (running) since ...
   Main PID: ...
      Tasks: ...
     Memory: ...
        CPU: ...

🔑 Step 10: Get Admin Password

sudo journalctl -u monsterzgophish -n 100 | grep -i password

Expected output:

time="..." level=info msg="Please login with the username admin and the password generated password"
🎯 Copy the generated password — you'll need it to log in.


✅ Final Test
🌍 Open Your Browser

Navigate to:

https://admin.alphalaef.ca

🔐 Login Credentials

Field	                  Value
Username	              admin
Password	              (the one from the command above)


✅ What to Expect

    🔒 Green padlock — SSL certificate is valid
    📊 Dashboard — Full Gophish admin panel loaded
    🟢 Everything working — Ready for campaigns

📚 Useful Commands (Save These)

Command	                                      Purpose
sudo systemctl restart monsterzgophish	    🔄 Restart Gophish
sudo journalctl -u monsterzgophish -f	      📋 Follow Gophish logs
sudo systemctl status nginx	                🌐 Check Nginx status
sudo certbot certificates	                  📜 Check SSL certificates
sudo systemctl stop monsterzgophish	        ⏹️ Stop Gophish
sudo systemctl start monsterzgophish	      ▶️ Start Gophish
sudo nginx -t	                              ✅ Test Nginx config

🛠️ Troubleshooting

Symptom	Likely                           Cause	                          Solution
❌ Cannot reach admin.alphalaef.ca	    DNS not propagated	              Wait or check A records
❌ No green padlock	                    SSL issue	                        Re-run sudo certbot certificates
❌ Nginx won't start	                  Config error	                    Run sudo nginx -t to debug
❌ Gophish won't start	                Port in use	                      sudo fuser -k 3333/tcp then restart
❌ 502 Bad Gateway	                    Gophish not running	              Check sudo systemctl status monsterzgophish
❌ Login page not loading              	Wrong proxy port	                Verify config.json admin port = 3333


📖 Keeping It Updated

cd /opt/monsterzgophish
git pull
go build
sudo systemctl restart monsterzgophish


🔗 Quick Links

Resource	                      URL
🌐 MonsterzGoPhish Repo	        github.com/officialmonsterz/monsterzgophish
📧 Contact	                    shapads@tutamail.com
💬 Telegram	                    t.me/officialmonsterz
🐙 GitHub	                      github.com/officialmonsterz




╔═══════════════════════════════════════════════════════════════╗
║                                                               ║
║    🎯 You are now fully set up. Happy Hacking! 🎯              ║
║                                                               ║
╚═══════════════════════════════════════════════════════════════╝


🏆 Credits

	
💬 Telegram	      t.me/officialmonsterz
🐙 GitHub	        github.com/officialmonsterz
📧 Email	        shapads@tutamail.com

Built with ❤️ for the security community

