# FINAL AWS DEPLOYMENT STEPS — Cloud-Based To-Do List Manager (MERN Stack)

---

# BACKEND SERVER COMMANDS (EC2 Instance 1)

```bash id="x1z0eg"
# Connect to backend EC2
chmod 400 blog.pem
ssh -i blog.pem ubuntu@BACKEND_PUBLIC_IP



# Create backend environment file
nano .env
```

---

# Add inside backend `.env`

```env id="s3a7l0"
PORT=5000

MONGODB_URI=mongodb+srv://YOUR_USERNAME:YOUR_PASSWORD@cluster.mongodb.net/tododb?retryWrites=true&w=majority
```

Example:

```env id="hpx44z"
PORT=5000

MONGODB_URI=mongodb+srv://vivek:password123@cluster0.mongodb.net/tododb?retryWrites=true&w=majority
```

---

# Enable PM2 after reboot
pm2 startup
```

Run the generated command from:

```bash id="55q0kq"
pm2 startup
```

Then again:

```bash id="97y8d9"
pm2 save
```

---

# FRONTEND SERVER COMMANDS (EC2 Instance 2)

```bash id="d9jv6j"
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

```env id="m7g0mv"
VITE_API_URL=http://BACKEND_PUBLIC_IP:5000/api
```

Example:

```env id="kq1l4m"
VITE_API_URL=http://13.60.25.200:5000/api
```

---

# Save `.env`

```text id="93b3y7"
CTRL + O
ENTER
CTRL + X
```

---

# Continue Frontend Setup

```bash id="r3pbxk"
# Build frontend for production
npm run build

# Remove old nginx files

---

# FINAL ACCESS

## Frontend Website

```text id="gv0ts7"
http://FRONTEND_PUBLIC_IP
```

Example:

```text id="6c1pj8"
http://16.170.232.103
```

---

## Backend API

```text id="lb7ruq"
http://BACKEND_PUBLIC_IP:5000/api
```

Example:

```text id="kxjlwm"
http://13.60.25.200:5000/api
```

---

# IMPORTANT NOTES

## Backend `.env`

Use:

```env id="5z4wlh"
MONGODB_URI=
```

because this project uses:

```js id="4mbry2"
process.env.MONGODB_URI
```

---

## Frontend `.env`

Use:

```env id="gbf19g"
VITE_API_URL=http://BACKEND_PUBLIC_IP:5000/api
```


because Vite embeds `.env` values during build time.
