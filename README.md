# Project Execution Steps
## Setting up the Server
1. **Created an AWS EC2 Instance** 
   - Used Ubuntu
   - Opened ports: 
     - **22** (SSH) 
     - **80** (HTTP) 
     - **443** (HTTPS)
2. **Connected to my Server**
   ```
   ssh -i my-key.pem ubuntu@my_server_ip
   ```
---
## Installed Dependencies
1. **Updated Packages**
   ```
   sudo apt update && sudo apt upgrade -y
   ```
2. **Installed Node.js & npm**
   ```
   sudo apt install nodejs npm -y
   ```
3. **Installed Git**
   sudo apt install git -y
   ```
---
## Created my Node.js App
1. **Set up my files and directories**
   ```
   mkdir my-node-app
   cd my-node-app
   nano server.js
   mkdir public
   nano index.html
   nano style.css
   ```
2. **Installed dependencies**
   ```
   npm install
   ```
3. **Tested my app**
   ```
   node server.js
   ```
---
## Used a Process Manager to run my App
1. **Installed PM2**
   ```
   sudo npm install -g pm2
   ```
2. **Started my app**
   ```
   pm2 start app.js
   ```
3. **Enabled PM2 Startup Script**
   ```
   pm2 startup
   pm2 save
   ```
---
## Installed & Configured NGINX
1. **Installed NGINX**
   ```
   sudo apt install nginx -y
   ```
2. **Started & Enabled NGINX**
   ```
   sudo systemctl start nginx
   sudo systemctl enable nginx
   ```
3. **Configured a Reverse Proxy**
   ```
   sudo nano /etc/nginx/sites-available/blueridge.twilightparadox.com
   ```
   ```
   # HTTP access via public IP
server {
    listen 80;
    server_name 18.134.146.129;

    location / {
        proxy_pass http://localhost:3000;  # Assuming Node.js is running on port 3000
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }

    access_log /var/log/nginx/blueridge_ip_access.log;
    error_log /var/log/nginx/blueridge_ip_error.log;
}

# HTTP access via domain (redirect to HTTPS)
server {
    listen 80;
    server_name blueridge.twilightparadox.com;

    location / {
        return 301 https://$host$request_uri;
    }
}

# HTTPS access via domain
server {
    listen 443 ssl;
    server_name blueridge.twilightparadox.com;

    ssl_certificate /etc/letsencrypt/live/blueridge.twilightparadox.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/blueridge.twilightparadox.com/privkey.pem;

    include /etc/letsencrypt/options-ssl-nginx.conf;
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }

    access_log /var/log/nginx/blueridge_domain_access.log;
    error_log /var/log/nginx/blueridge_domain_error.log;
}
   ```
4. **Enabled the Config**
   ```
   sudo ln -s /etc/nginx/sites-available/blueridge.twilightparadox.com /etc/nginx/sites-enabled/
   ```
5. **Tested the NGINX Config**
   ```
   sudo nginx -t
   ```
6. **Restarted NGINX**
   ```
   sudo systemctl restart nginx
   ```
---
## Secure my App with HTTPS (Let’s Encrypt)
1. **Installed Certbot**
   ```
   sudo apt install certbot python3-certbot-nginx -y
   ```
2. **Obtained & Installed SSL**
   ```
   sudo certbot --nginx -d blueridge.twilightparadox.com -d www.blueridge.twilightparadox.com
   ```
3. **Pushed to GitHub**
```
git init
git status
git commit -m "first commit"
git remote add origin
git push origin master
---
