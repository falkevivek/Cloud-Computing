# FINAL AWS DEPLOYMENT STEPS — Blog Application (MERN Stack)


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
