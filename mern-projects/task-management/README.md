
# Create backend environment file
nano .env
```

---

# Add inside backend `.env`

```env
PORT=5000

MONGODB_URI=mongodb+srv://YOUR_USERNAME:YOUR_PASSWORD@cluster.mongodb.net/taskdb?retryWrites=true&w=majority

FRONTEND_URL=http://FRONTEND_PUBLIC_IP
```

Example:

```env
PORT=5000

MONGODB_URI=mongodb+srv://vivek:password123@cluster0.mongodb.net/taskdb?retryWrites=true&w=majority

FRONTEND_URL=http://16.170.232.103
```

---


```bash
sudo env PATH=$PATH:/usr/bin pm2 startup systemd -u ubuntu --hp /home/ubuntu
```

Then again:

```bash
pm2 save
```

---


# Add inside frontend `.env`

```env
VITE_API_URL=http://BACKEND_PUBLIC_IP:5000/api
```

Example:

```env
VITE_API_URL=http://13.60.25.200:5000/api
```

---

# Save `.env`

```text
CTRL + O
ENTER
CTRL + X
```

---
