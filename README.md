# Project Execution Steps

🔗 [Live Landing Page](https://blueridge.twilightparadox.com)  
🌐 [Access via Public IP](http://18.134.146.129)

---

## 1. Setting Up the Server

1. **Created an AWS EC2 Instance**
   - OS: Ubuntu
   - Opened ports:
     - **22** (SSH)
     - **80** (HTTP)
     - **443** (HTTPS)

2. **Connected to the Server**
   ```bash
   ssh -i my-key.pem ubuntu@your_server_ip
   ```

---

## 2. Installed Dependencies

1. **Updated System Packages**
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```

2. **Installed Node.js and npm**
   ```bash
   sudo apt install nodejs npm -y
   ```

3. **Installed Git**
   ```bash
   sudo apt install git -y
   ```

---

## 3. Created the Node.js App

1. **Set Up Files and Directories**
   ```bash
   mkdir my-node-app
   cd my-node-app
   nano server.js
   mkdir public
   nano public/index.html
   nano public/style.css
   ```

2. **Initialized npm and Installed Dependencies**
   ```bash
   npm init -y
   npm install
   ```

3. **Tested the App**
   ```bash
   node server.js
   ```

---

## 4. Used a Process Manager to Run the App

1. **Installed PM2**
   ```bash
   sudo npm install -g pm2
   ```

2. **Started the App with PM2**
   ```bash
   pm2 start server.js
   ```

3. **Enabled PM2 Startup on Boot**
   ```bash
   pm2 startup
   pm2 save
   ```

---

## 5. Installed & Configured NGINX

1. **Installed NGINX**
   ```bash
   sudo apt install nginx -y
   ```

2. **Started and Enabled NGINX**
   ```bash
   sudo systemctl start nginx
   sudo systemctl enable nginx
   ```

3. **Configured Reverse Proxy**
   ```bash
   sudo nano /etc/nginx/sites-available/blueridge.twilightparadox.com
   ```

   **Sample Configuration:**
   ```nginx
   # HTTP access via public IP (for testing)
   server {
       listen 80;
       server_name 18.134.146.129;

       location / {
           proxy_pass http://localhost:3000;
           proxy_http_version 1.1;
           proxy_set_header Upgrade $http_upgrade;
           proxy_set_header Connection 'upgrade';
           proxy_set_header Host $host;
           proxy_cache_bypass $http_upgrade;
       }

       access_log /var/log/nginx/blueridge_ip_access.log;
       error_log /var/log/nginx/blueridge_ip_error.log;
   }

   # HTTP to HTTPS redirection for domain
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

4. **Enabled the NGINX Configuration**
   ```bash
   sudo ln -s /etc/nginx/sites-available/blueridge.twilightparadox.com /etc/nginx/sites-enabled/
   ```

5. **Tested the NGINX Configuration**
   ```bash
   sudo nginx -t
   ```

6. **Restarted NGINX**
   ```bash
   sudo systemctl restart nginx
   ```

---

## 6. Secured the App with HTTPS (Let’s Encrypt)

1. **Installed Certbot**
   ```bash
   sudo apt install certbot python3-certbot-nginx -y
   ```

2. **Obtained and Installed SSL Certificate**
   ```bash
   sudo certbot --nginx -d blueridge.twilightparadox.com -d www.blueridge.twilightparadox.com
   ```

---

## 7. Pushed Project to GitHub

1. **Initialized Git and Pushed Code**
   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   git remote add origin https://github.com/Vickie17/my-node-app.git
   git push -u origin master
   ```

---



