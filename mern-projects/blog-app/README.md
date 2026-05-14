# FINAL AWS DEPLOYMENT STEPS — Blog Application (MERN Stack)

---

# BACKEND SERVER COMMANDS (EC2 Instance 1)

```bash id="xgk5tv"
# Connect to backend EC2
chmod 400 blog.pem
ssh -i blog.pem ubuntu@BACKEND_PUBLIC_IP

# Update Ubuntu
sudo apt update -y

# Install Git and Curl
sudo apt install git curl -y

# Install Node.js
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt install nodejs -y

# Check installation
node -v
npm -v

# Clone GitHub repository
git clone https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.git

# Open repository
cd YOUR_REPOSITORY

# Open backend folder
cd backend

# Install backend packages
npm install

# Install PM2 globally
sudo npm install -g pm2

# Create backend environment file
nano .env
```

---

# Add inside backend `.env`

```env id="5itjlwm"
PORT=5000

MONGODB_URI=mongodb+srv://vkbhamare26_db_user:lEML4pbtAI6KrMfo@cluster0.sspjpor.mongodb.net/?appName=Cluster0
```

---

# Save `.env`

```text id="29q6a1"
CTRL + O
ENTER
CTRL + X
```

---

# Continue Backend Setup

```bash id="n3xz6o"
# Start backend using PM2
pm2 start src/server.js --name blog-backend

# Check backend logs
pm2 logs

# Show running PM2 processes
pm2 list

# Save PM2 process list
pm2 save

# Enable PM2 after reboot
pm2 startup
```

Run the generated command from:

```bash id="21jzrt"
pm2 startup
```

Then again:

```bash id="0f7d7o"
pm2 save
```

---

# FRONTEND SERVER COMMANDS (EC2 Instance 2)

```bash id="v8jlwm"
# Connect to frontend EC2
chmod 400 blog.pem
ssh -i blog.pem ubuntu@FRONTEND_PUBLIC_IP

# Update Ubuntu
sudo apt update -y

# Install Git, Curl and Nginx
sudo apt install git curl nginx -y

# Install Node.js
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt install nodejs -y

# Check installation
node -v
npm -v

# Clone GitHub repository
git clone https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.git

# Open repository
cd YOUR_REPOSITORY

# Open frontend folder
cd frontend

# Install frontend packages
npm install

# Create frontend environment file
nano .env
```

---

# Add inside frontend `.env`

```env id="q0x7rj"
VITE_API_URL=http://BACKEND_PUBLIC_IP:5000/api
```

Example:

```env id="pw0f7h"
VITE_API_URL=http://13.60.25.200:5000/api
```

---

# Save `.env`

```text id="1db4xw"
CTRL + O
ENTER
CTRL + X
```

---

# Continue Frontend Setup

```bash id="gxkr1n"
# Build frontend for production
npm run build

# Remove old nginx files
sudo rm -rf /var/www/html/*

# Copy React build files to nginx folder
sudo cp -r dist/* /var/www/html/

# Start nginx
sudo systemctl start nginx

# Enable nginx after reboot
sudo systemctl enable nginx

# Restart nginx
sudo systemctl restart nginx

# Check nginx status
sudo systemctl status nginx
```

---

# SECURITY GROUP CONFIGURATION

## Use SAME Security Group for Both EC2 Instances

| Type       | Port | Source   |
| ---------- | ---- | -------- |
| SSH        | 22   | My IP    |
| HTTP       | 80   | Anywhere |
| Custom TCP | 5000 | Anywhere |

---

# FINAL ACCESS

## Frontend Website

```text id="40jlwm"
http://FRONTEND_PUBLIC_IP
```

Example:

```text id="xtbysh"
http://16.170.232.103
```

---

## Backend API

```text id="znc5vk"
http://BACKEND_PUBLIC_IP:5000/api
```

Example:

```text id="8odbh4"
http://13.60.25.200:5000/api
```

---

# IMPORTANT NOTES

## Backend `.env`

Use:

```env id="4xgnpd"
MONGODB_URI=
```

because this project uses:

```js id="wpgksd"
process.env.MONGODB_URI
```

---

## Frontend `.env`

Use:

```env id="jlwm3f"
VITE_API_URL=http://BACKEND_PUBLIC_IP:5000/api
```

because backend routes use `/api`.

---

# If Frontend Cannot Connect To Backend

Rebuild frontend again:

```bash id="vjlwm0"
npm run build
sudo rm -rf /var/www/html/*
sudo cp -r dist/* /var/www/html/
sudo systemctl restart nginx
```

because Vite embeds `.env` values during build time.
