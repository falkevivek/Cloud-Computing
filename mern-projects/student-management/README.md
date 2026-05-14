# FINAL AWS DEPLOYMENT STEPS — Student Record Management System (MERN Stack)

---

# BACKEND SERVER COMMANDS (EC2 Instance 1)

```bash id="g1qk2m"
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

```env id="a8s4vk"
PORT=5000

MONGODB_URI=mongodb+srv://vkbhamare26_db_user:lEML4pbtAI6KrMfo@cluster0.sspjpor.mongodb.net/?appName=Cluster0
```

---

# Save `.env`

```text id="r2k7zq"
CTRL + O
ENTER
CTRL + X
```

---

# Continue Backend Setup

```bash id="u9v3jx"
# Start backend using PM2
pm2 start src/server.js --name student-backend

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

```bash id="m6p1hf"
pm2 startup
```

Then again:

```bash id="n3x7bt"
pm2 save
```

---

# FRONTEND SERVER COMMANDS (EC2 Instance 2)

```bash id="z5w2lm"
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

```env id="t4n8wp"
VITE_API_URL=http://BACKEND_PUBLIC_IP:5000/api
```

Example:

```env id="k9u3dr"
VITE_API_URL=http://13.60.25.200:5000/api
```

---

# Save `.env`

```text id="f7q1mc"
CTRL + O
ENTER
CTRL + X
```

---

# Continue Frontend Setup

```bash id="y2c6vb"
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

```text id="j7v4nb"
http://FRONTEND_PUBLIC_IP
```

Example:

```text id="e8m2qt"
http://16.170.232.103
```

---

## Backend API

```text id="u1p9dx"
http://BACKEND_PUBLIC_IP:5000/api
```

Example:

```text id="s4l8kc"
http://13.60.25.200:5000/api
```

---

# IMPORTANT NOTES

## Backend `.env`

Use:

```env id="q6z3wr"
MONGODB_URI=
```

because this project uses:

```js id="v8x1mt"
process.env.MONGODB_URI
```

---

## Frontend `.env`

Use:

```env id="n5b7qy"
VITE_API_URL=http://BACKEND_PUBLIC_IP:5000/api
```

because backend routes use `/api`.

---

# If Frontend Cannot Connect To Backend

Rebuild frontend again:

```bash id="c3r9lu"
npm run build
sudo rm -rf /var/www/html/*
sudo cp -r dist/* /var/www/html/
sudo systemctl restart nginx
```

because Vite embeds `.env` values during build time.
