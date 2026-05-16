
---

```env id="6ph93q"
PORT=5000

MONGODB_URI=mongodb+srv://vkbhamare26_db_user:lEML4pbtAI6KrMfo@cluster0.sspjpor.mongodb.net/?appName=Cluster0
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
