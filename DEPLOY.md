# 🚀 CarEase Backend Deployment Guide

## 📋 **COMPLETE BACKEND DEPLOYMENT (45 MINUTES)**

This guide will help you deploy your CarEase backend with database.

---

## ✅ **WHAT YOU'RE DEPLOYING:**

- **PostgreSQL Database** (Supabase - Free tier)
- **Node.js/Express API** (Railway - Free tier)
- **Complete features:**
  - User authentication (JWT)
  - Student verification
  - Car listings
  - Booking system with 50% student discount
  - Payment processing (Stripe)
  - Reviews system
  - Admin panel

---

## 🗄️ **STEP 1: DEPLOY DATABASE (15 minutes)**

### **1.1 Create Supabase Account**

1. Go to: **https://supabase.com**
2. Click **"Start your project"**
3. Sign up with **GitHub** (easiest)
4. Verify your email

### **1.2 Create New Project**

1. Click **"New Project"**
2. Fill in:
   - **Name:** CarEase
   - **Database Password:** Create a strong password (SAVE THIS!)
   - **Region:** Choose **Sydney** (closest to Melbourne)
   - **Pricing Plan:** Free tier is perfect
3. Click **"Create new project"**
4. ⏰ **Wait 2-3 minutes** for database to provision

### **1.3 Run Database Schema**

1. In your Supabase dashboard, click **"SQL Editor"** (left sidebar)
2. Click **"New query"**
3. **Open the `schema.sql` file** from this folder
4. **Copy ALL the SQL** (entire file)
5. **Paste** into the Supabase SQL editor
6. Click **"Run"** (or press Ctrl+Enter)
7. ✅ You should see: **"Success. No rows returned"**

### **1.4 Get Database Connection String**

1. Click **"Settings"** (left sidebar, bottom)
2. Click **"Database"**
3. Scroll down to **"Connection string"**
4. Select **"URI"** tab
5. **Copy the connection string** - looks like:
   ```
   postgresql://postgres:[YOUR-PASSWORD]@db.xxx.supabase.co:5432/postgres
   ```
6. **Replace `[YOUR-PASSWORD]`** with the password you created
7. ✅ **SAVE THIS CONNECTION STRING!** You'll need it soon

---

## 🚂 **STEP 2: DEPLOY BACKEND API (20 minutes)**

### **2.1 Create Railway Account**

1. Go to: **https://railway.app**
2. Click **"Start a New Project"**
3. Sign in with **GitHub**
4. Authorize Railway

### **2.2 Create New Project**

1. Click **"New Project"**
2. Select **"Deploy from GitHub repo"**
3. If this is your first time:
   - Click **"Configure GitHub App"**
   - Select your repositories
   - Give Railway access

**WAIT!** We need to push backend to GitHub first.

---

## 📦 **STEP 2.5: PUSH TO GITHUB (10 minutes)**

### **Option A: Quick Deploy (Recommended)**

Since we already have the code, let's use Railway's direct upload:

1. In Railway dashboard, click **"New Project"**
2. Select **"Empty Project"**
3. Click **"Deploy"** → **"Empty Service"**
4. A new service will be created
5. In the service, click **"Settings"**
6. Under **"Source"**, we'll manually upload

**Actually, let's use the CLI method instead:**

### **Option B: Railway CLI (Easier)**

1. **Install Railway CLI:**
   ```bash
   npm install -g @railway/cli
   ```

2. **In this folder**, run:
   ```bash
   railway login
   ```

3. **Initialize project:**
   ```bash
   railway init
   ```

4. **Link to new project:**
   ```bash
   railway link
   ```

5. **Deploy:**
   ```bash
   railway up
   ```

**OR Use GitHub (Best):**

### **Option C: Using GitHub (RECOMMENDED)**

I'll create a quick guide for GitHub...

Actually, let's use the **SIMPLEST method**:

---

## 🎯 **EASIEST METHOD: Manual Railway Deploy**

### **Step 1: Create GitHub Repo**

1. Go to: **https://github.com/new**
2. Name: **carease-backend**
3. Make it **Private**
4. Don't initialize with README
5. Click **"Create repository"**

### **Step 2: Push Code to GitHub**

In your terminal, in this folder:

```bash
git init
git add .
git commit -m "Initial CarEase backend"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/carease-backend.git
git push -u origin main
```

### **Step 3: Deploy to Railway**

1. Go back to Railway dashboard
2. **"New Project"** → **"Deploy from GitHub repo"**
3. Select **carease-backend** repo
4. Railway will auto-detect it's a Node.js app
5. Click **"Deploy Now"**

### **Step 4: Add Environment Variables**

1. In Railway, click on your deployed service
2. Click **"Variables"** tab
3. Add these variables:

```
DATABASE_URL=postgresql://postgres:[password]@db.xxx.supabase.co:5432/postgres
JWT_SECRET=carease_secret_key_2025_change_this_in_production
FRONTEND_URL=https://carease.com.au
NODE_ENV=production
PORT=5000
STRIPE_SECRET_KEY=sk_test_your_stripe_key_here
STRIPE_WEBHOOK_SECRET=whsec_your_webhook_secret
```

**Important:**
- Replace `DATABASE_URL` with your Supabase connection string
- Change `JWT_SECRET` to something random and secure
- We'll add Stripe keys later

4. Click **"Save"**

### **Step 5: Get Your API URL**

1. In Railway, click **"Settings"** tab
2. Scroll to **"Domains"**
3. Click **"Generate Domain"**
4. You'll get a URL like: `https://carease-production-xxxx.up.railway.app`
5. ✅ **COPY THIS URL!** This is your backend API URL

---

## 🧪 **STEP 3: TEST YOUR BACKEND (5 minutes)**

### **3.1 Test Health Endpoint**

1. Open browser
2. Go to: `https://your-railway-url.up.railway.app/api/health`
3. You should see:
   ```json
   {
     "status": "ok",
     "message": "CarEase API is running",
     "timestamp": "2025-12-23T07:15:00.000Z"
   }
   ```
4. ✅ If you see this, **BACKEND IS LIVE!**

### **3.2 Test API Root**

1. Go to: `https://your-railway-url.up.railway.app/`
2. You should see all available endpoints
3. ✅ Perfect!

---

## 🔗 **STEP 4: CONNECT TO FRONTEND (10 minutes)**

Now we need to update the frontend to call your backend API.

### **4.1 Update Frontend**

In your frontend HTML file, add this at the top of the `<script>` section:

```javascript
// API Configuration
const API_URL = 'https://your-railway-url.up.railway.app/api';

// API Helper Functions
async function apiCall(endpoint, method = 'GET', data = null) {
  const options = {
    method,
    headers: {
      'Content-Type': 'application/json',
    },
  };
  
  const token = localStorage.getItem('token');
  if (token) {
    options.headers['Authorization'] = `Bearer ${token}`;
  }
  
  if (data) {
    options.body = JSON.stringify(data);
  }
  
  try {
    const response = await fetch(`${API_URL}${endpoint}`, options);
    const result = await response.json();
    return result;
  } catch (error) {
    console.error('API Error:', error);
    throw error;
  }
}

// Update registration function
async function handleRegister(formData) {
  try {
    const result = await apiCall('/auth/register', 'POST', formData);
    if (result.token) {
      localStorage.setItem('token', result.token);
      localStorage.setItem('user', JSON.stringify(result.user));
      showPage('dashboard');
    }
  } catch (error) {
    alert('Registration failed: ' + error.message);
  }
}

// Update login function
async function handleLogin(email, password) {
  try {
    const result = await apiCall('/auth/login', 'POST', { email, password });
    if (result.token) {
      localStorage.setItem('token', result.token);
      localStorage.setItem('user', JSON.stringify(result.user));
      showPage('dashboard');
    }
  } catch (error) {
    alert('Login failed: ' + error.message);
  }
}
```

### **4.2 Redeploy Frontend**

1. Save updated HTML file
2. Go to Netlify
3. Drag and drop updated file
4. ✅ Frontend now connected to backend!

---

## ✅ **COMPLETE DEPLOYMENT CHECKLIST:**

- [ ] Supabase account created
- [ ] Database schema deployed
- [ ] Connection string saved
- [ ] Railway account created
- [ ] Backend code pushed to GitHub
- [ ] Backend deployed to Railway
- [ ] Environment variables set
- [ ] Backend API URL obtained
- [ ] Health endpoint tested
- [ ] Frontend updated with API URL
- [ ] Frontend redeployed
- [ ] **FULL STACK LIVE!** 🎉

---

## 🎯 **YOUR LIVE URLS:**

- **Frontend:** https://carease.com.au
- **Backend API:** https://your-app.up.railway.app
- **Database:** Supabase (managed internally)

---

## 📱 **WHAT YOU CAN DO NOW:**

1. **Register a new account** on your website
2. **Login** with your credentials
3. **Add a test car** (as a host)
4. **Make a test booking** (as a renter)
5. **See everything saving to database!**

---

## 💰 **COSTS:**

- **Domain:** $15/year (already paid)
- **Supabase:** $0 (free tier - 500MB database, 50k users)
- **Railway:** $0 (free tier - $5 credit/month)
- **Total:** **$0/month** 🎉

---

## 🚨 **TROUBLESHOOTING:**

### **Backend won't deploy:**
- Check Railway logs: Dashboard → Service → Logs
- Verify all environment variables are set
- Make sure package.json is in root folder

### **Database connection fails:**
- Verify Supabase connection string is correct
- Check password has no special characters that need escaping
- Test connection in Supabase SQL editor first

### **API returns errors:**
- Check Railway logs for error messages
- Verify DATABASE_URL is set correctly
- Test individual endpoints with Postman

---

## 🆘 **NEED HELP?**

**Common Issues:**

1. **"Cannot connect to database"**
   - Check DATABASE_URL in Railway variables
   - Verify Supabase is running (green status)

2. **"Port already in use"**
   - Railway handles ports automatically
   - Make sure PORT variable is set or removed

3. **"Module not found"**
   - Run `npm install` locally
   - Push updated package-lock.json to GitHub
   - Redeploy

---

## 📚 **API ENDPOINTS:**

### **Authentication:**
- POST `/api/auth/register` - Register new user
- POST `/api/auth/login` - Login user
- GET `/api/auth/me` - Get current user (requires token)

### **Vehicles:**
- GET `/api/vehicles` - Get all vehicles
- GET `/api/vehicles/:id` - Get vehicle by ID
- POST `/api/vehicles` - Create vehicle (requires auth)
- PUT `/api/vehicles/:id` - Update vehicle
- DELETE `/api/vehicles/:id` - Delete vehicle

### **Bookings:**
- GET `/api/bookings` - Get user's bookings
- POST `/api/bookings` - Create booking
- GET `/api/bookings/:id` - Get booking details
- PUT `/api/bookings/:id/status` - Update booking status

### **Reviews:**
- GET `/api/reviews/vehicle/:id` - Get vehicle reviews
- POST `/api/reviews` - Create review
- GET `/api/reviews/user/:id` - Get user's reviews

### **Users:**
- GET `/api/users/profile` - Get user profile
- PUT `/api/users/profile` - Update profile
- PUT `/api/users/student-verify` - Submit student verification

---

## 🎉 **CONGRATULATIONS!**

Your backend is now LIVE and connected to your frontend!

You have a fully functional car rental platform with:
- ✅ User registration & login
- ✅ Student verification (50% discount)
- ✅ Car listings
- ✅ Booking system
- ✅ Reviews
- ✅ Payment processing (ready for Stripe)

**Next steps:**
1. Add real cars
2. Test bookings
3. Set up Stripe for payments
4. Launch to your first users!

**YOU DID IT!** 🚀💪🎊
