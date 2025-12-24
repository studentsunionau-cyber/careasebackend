# ⚡ HEROKU QUICK START - 20 MINUTES!

Follow these exact steps to get your backend live on Heroku.

---

## 🚀 STEP-BY-STEP (NO SKIPPING!)

### **STEP 1: Supabase Database (10 mins)**

1. Go to https://supabase.com → Sign up with GitHub
2. Click "New Project"
3. Name: **CarEase** | Password: **(SAVE IT!)** | Region: **Sydney**
4. Wait 2 minutes
5. Click "SQL Editor" → "New Query"
6. Open `schema.sql` from this folder → Copy ALL → Paste → Click "Run"
7. Click "Settings" → "Database" → Copy connection string URI
8. Replace `[YOUR-PASSWORD]` with your actual password
9. ✅ **SAVE THIS CONNECTION STRING!**

---

### **STEP 2: Heroku Setup (5 mins)**

1. Go to https://heroku.com → Sign up (free)
2. Verify email
3. Download Heroku CLI: https://devcenter.heroku.com/articles/heroku-cli
4. Install it
5. Open terminal/command prompt
6. Type: `heroku login` → Press any key → Login in browser

---

### **STEP 3: Deploy (5 mins)**

In terminal, navigate to this folder:

```bash
cd carease-backend
```

Login to Heroku (if not done):
```bash
heroku login
```

Create app:
```bash
heroku create carease-api
```

*(If name taken, try: carease-backend-2025)*

Set environment variables:
```bash
heroku config:set DATABASE_URL="YOUR_SUPABASE_CONNECTION_STRING_HERE"

heroku config:set JWT_SECRET="carease_secret_2025_change_this"

heroku config:set FRONTEND_URL="https://carease.com.au"

heroku config:set NODE_ENV="production"
```

**Replace `DATABASE_URL` with your actual Supabase connection string!**

Initialize git (if not done):
```bash
git init
git add .
git commit -m "Deploy to Heroku"
```

Deploy:
```bash
git push heroku main
```

*(Or `git push heroku master` if your branch is master)*

Wait 2-3 minutes...

✅ **DEPLOYED!**

Get your URL:
```bash
heroku info
```

Copy the "Web URL" - something like:
`https://carease-api-xxxxx.herokuapp.com`

---

### **STEP 4: Test (2 mins)**

Open browser, go to:
```
https://your-app-name.herokuapp.com/api/health
```

Should see:
```json
{"status": "ok", "message": "CarEase API is running"}
```

✅ **IT WORKS!**

---

## 🔗 CONNECT TO FRONTEND

In your frontend HTML, add this at the top of `<script>`:

```javascript
const API_URL = 'https://your-heroku-app.herokuapp.com/api';

async function apiCall(endpoint, method = 'GET', data = null) {
  const options = {
    method,
    headers: {'Content-Type': 'application/json'},
  };
  
  const token = localStorage.getItem('token');
  if (token) options.headers['Authorization'] = `Bearer ${token}`;
  
  if (data) options.body = JSON.stringify(data);
  
  const response = await fetch(`${API_URL}${endpoint}`, options);
  return await response.json();
}

// Use like this:
// const result = await apiCall('/auth/register', 'POST', {email, password, ...});
```

Update your forms to call these APIs!

Redeploy frontend to Netlify.

✅ **FULL STACK CONNECTED!**

---

## 💰 COST

- Heroku Eco: **$5/month**
- Supabase: **$0/month** (free tier)
- **Total: $5/month**

---

## 🆘 IF SOMETHING BREAKS

View logs:
```bash
heroku logs --tail
```

Restart app:
```bash
heroku restart
```

Check config:
```bash
heroku config
```

---

## ✅ YOU'RE DONE!

**Your API is live at:** https://your-app.herokuapp.com

**Test endpoints:**
- `/api/health` - Health check
- `/api/auth/register` - Register user
- `/api/auth/login` - Login user
- `/api/vehicles` - Get cars
- `/api/bookings` - Get bookings

**Now connect your frontend and START TESTING!** 🎉
