# FINAL AWS DEPLOYMENT STEPS — E-Commerce Web Application (MERN Stack)

---

# BACKEND SERVER COMMANDS (EC2 Instance 1)

```bash id="d9sk2m"
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

```env id="6ph93q"
PORT=5000

MONGODB_URI=mongodb+srv://vkbhamare26_db_user:lEML4pbtAI6KrMfo@cluster0.sspjpor.mongodb.net/?appName=Cluster0
```

---

# Save `.env`

```text id="e5twur"
CTRL + O
ENTER
CTRL + X
```

---

# Continue Backend Setup

```bash id="xtm9kv"
# Seed sample products (run only once)
npm run seed

# Start backend using PM2
pm2 start src/server.js --name ecommerce-backend

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

```bash id="55c4v2"
pm2 startup
```

Then again:

```bash id="8nq3lt"
pm2 save
```

---

# FRONTEND SERVER COMMANDS (EC2 Instance 2)

```bash id="k4r0zn"
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

```env id="sj4z7e"
VITE_API_URL=http://BACKEND_PUBLIC_IP:5000/api
```

Example:

```env id="zytq0v"
VITE_API_URL=http://13.60.25.200:5000/api
```

---

# Save `.env`

```text id="8mq4wc"
CTRL + O
ENTER
CTRL + X
```

---

# Continue Frontend Setup

```bash id="jxm0uw"
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

```text id="i7u1kl"
http://FRONTEND_PUBLIC_IP
```

Example:

```text id="u8c4fm"
http://16.170.232.103
```

---

## Backend API

```text id="jlwm6o"
http://BACKEND_PUBLIC_IP:5000/api
```

Example:

```text id="jlwm6p"
http://13.60.25.200:5000/api
```

---

# IMPORTANT NOTES

## Backend `.env`

Use:

```env id="jlwm6q"
MONGODB_URI=
```

because this project uses:

```js id="jlwm6r"
process.env.MONGODB_URI
```

---

## Frontend `.env`

Use:

```env id="jlwm6s"
VITE_API_URL=http://BACKEND_PUBLIC_IP:5000/api
```

because backend routes use `/api`.

---

# If Frontend Cannot Connect To Backend

Rebuild frontend again:

```bash id="jlwm6t"
npm run build
sudo rm -rf /var/www/html/*
sudo cp -r dist/* /var/www/html/
sudo systemctl restart nginx
```

because Vite embeds `.env` values during build time.
