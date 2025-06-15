
# ✅ Node.js Server Setup & Deployment Guide (with NGINX)

## 📌 1️⃣ Provision the Server

1. **Create an EC2 Instance (or VPS)**  
   - Use a Linux image (e.g., Ubuntu 22.04 LTS).
   - Open ports:  
     - **22** (SSH)  
     - **80** (HTTP)  
     - **443** (HTTPS)

2. **Connect to your Server**
   ```bash
   ssh -i /path/to/your-key.pem ubuntu@your_server_ip
   ```

---

## 📌 2️⃣ Update & Install Dependencies

1. **Update Packages**
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```

2. **Install Node.js & npm**
   ```bash
   sudo apt install nodejs npm -y
   ```
   Or use Node Version Manager (recommended):
   ```bash
   curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
   sudo apt-get install -y nodejs
   ```

3. **Install Git**
   ```bash
   sudo apt install git -y
   ```

---

## 📌 3️⃣ Clone & Setup your Node.js App

1. **Clone your repository**
   ```bash
   git clone https://github.com/your-username/your-repo.git
   cd your-repo
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Test your app**
   ```bash
   node app.js
   ```

---

## 📌 4️⃣ Run your Node App with a Process Manager

1. **Install PM2**
   ```bash
   sudo npm install -g pm2
   ```

2. **Start your app**
   ```bash
   pm2 start app.js
   ```

3. **Enable PM2 Startup Script**
   ```bash
   pm2 startup
   pm2 save
   ```

---

## 📌 5️⃣ Install & Configure NGINX

1. **Install NGINX**
   ```bash
   sudo apt install nginx -y
   ```

2. **Start & Enable NGINX**
   ```bash
   sudo systemctl start nginx
   sudo systemctl enable nginx
   ```

3. **Configure a Reverse Proxy**
   ```bash
   sudo nano /etc/nginx/sites-available/blueridge.twilightparadox.com
   ```

   Example config:
   ```nginx
   server {
       listen 80;
       server_name blueridge.twilightparadox.com www.blueridge.twilightparadox.com;

       location / {
           proxy_pass http://localhost:3000;
           proxy_http_version 1.1;
           proxy_set_header Upgrade $http_upgrade;
           proxy_set_header Connection 'upgrade';
           proxy_set_header Host $host;
           proxy_cache_bypass $http_upgrade;
       }
   }
   ```

4. **Enable the Config**
   ```bash
   sudo ln -s /etc/nginx/sites-available/blueridge.twilightparadox.com /etc/nginx/sites-enabled/
   ```

5. **Test the NGINX Config**
   ```bash
   sudo nginx -t
   ```

6. **Restart NGINX**
   ```bash
   sudo systemctl restart nginx
   ```

---

## 📌 6️⃣ Secure with HTTPS (Let’s Encrypt)

1. **Install Certbot**
   ```bash
   sudo apt install certbot python3-certbot-nginx -y
   ```

2. **Obtain & Install SSL**
   ```bash
   sudo certbot --nginx -d blueridge.twilightparadox.com -d www.blueridge.twilightparadox.com
   ```

3. **Verify Renewal**
   ```bash
   sudo certbot renew --dry-run
   ```

---

## 🎉 Done!

- Your Node.js app is running.
- NGINX is serving it securely via HTTPS.
- PM2 keeps it alive.

---

## ✅ Common Commands

| Purpose                | Command                                  |
|------------------------|------------------------------------------|
| Check NGINX status     | `sudo systemctl status nginx`            |
| Restart NGINX          | `sudo systemctl restart nginx`           |
| Restart your app with PM2 | `pm2 restart all`                    |
| View app logs          | `pm2 logs`                               |
| View PM2 process list  | `pm2 list`                               |
