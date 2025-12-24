# 🚀 CarEase Backend - HEROKU DEPLOYMENT

## 📋 **DEPLOY TO HEROKU (30 MINUTES)**

Complete guide to deploy your backend API to Heroku.

---

## ✅ **WHAT YOU'RE DEPLOYING:**

- **Database:** Supabase (PostgreSQL - Free)
- **Backend API:** Heroku (Node.js/Express - Free)
- **Features:** Full authentication, bookings, student discounts, payments

---

## 🗄️ **STEP 1: DEPLOY DATABASE (15 minutes)**

### **1.1 Create Supabase Account**

1. Go to: **https://supabase.com**
2. Click **"Start your project"**
3. Sign up with **GitHub**
4. Verify email

### **1.2 Create Database Project**

1. Click **"New Project"**
2. Fill in:
   - **Name:** CarEase
   - **Database Password:** Create strong password **(SAVE THIS!)**
   - **Region:** Sydney
   - **Plan:** Free
3. Click **"Create new project"**
4. ⏰ Wait 2-3 minutes

### **1.3 Run Database Schema**

1. Click **"SQL Editor"** (left sidebar)
2. Click **"New query"**
3. Open `schema.sql` from the backend folder
4. **Copy ALL the SQL**
5. **Paste** into Supabase editor
6. Click **"Run"**
7. ✅ Success! Database tables created

### **1.4 Get Connection String**

1. Click **"Settings"** → **"Database"**
2. Scroll to **"Connection string"**
3. Select **"URI"** tab
4. Copy the connection string:
   ```
   postgresql://postgres:[PASSWORD]@db.xxx.supabase.co:5432/postgres
   ```
5. **Replace `[PASSWORD]`** with your actual password
6. ✅ **SAVE THIS!** You'll need it for Heroku

---

## 🟣 **STEP 2: DEPLOY TO HEROKU (15 minutes)**

### **2.1 Create Heroku Account**

1. Go to: **https://heroku.com**
2. Click **"Sign up for free"**
3. Fill in details
4. Verify email
5. ✅ Account created!

### **2.2 Install Heroku CLI**

**On Windows:**
1. Download: https://devcenter.heroku.com/articles/heroku-cli
2. Run installer
3. Open Command Prompt
4. Type: `heroku --version`
5. ✅ Should show version number

**On Mac:**
```bash
brew tap heroku/brew && brew install heroku
```

**On Linux:**
```bash
curl https://cli-assets.heroku.com/install.sh | sh
```

### **2.3 Login to Heroku**

Open terminal/command prompt:

```bash
heroku login
```

- Press any key
- Browser opens
- Click **"Log in"**
- ✅ You're logged in!

### **2.4 Prepare Backend for Heroku**

We need to add a `Procfile`. In the backend folder, create a file named `Procfile` (no extension):

```
web: node server.js
```

Also, make sure `package.json` has the start script:

```json
{
  "scripts": {
    "start": "node server.js"
  }
}
```

✅ Backend is ready!

### **2.5 Create Heroku App**

In terminal, navigate to your backend folder:

```bash
cd carease-backend
```

Create Heroku app:

```bash
heroku create carease-api
```

**Note:** If `carease-api` is taken, try:
- `carease-backend`
- `carease-api-2025`
- `your-name-carease`

✅ App created! You'll get a URL like:
```
https://carease-api-xxxxx.herokuapp.com
```

### **2.6 Set Environment Variables**

Set your config vars:

```bash
heroku config:set DATABASE_URL="postgresql://postgres:[PASSWORD]@db.xxx.supabase.co:5432/postgres"

heroku config:set JWT_SECRET="carease_secret_key_change_this_2025"

heroku config:set FRONTEND_URL="https://carease.com.au"

heroku config:set NODE_ENV="production"
```

**Replace:**
- `DATABASE_URL` with your actual Supabase connection string
- `JWT_SECRET` with a random secure string

✅ Environment variables set!

### **2.7 Deploy to Heroku**

Initialize git (if not done):

```bash
git init
git add .
git commit -m "Initial commit"
```

Deploy:

```bash
git push heroku main
```

OR if your branch is named `master`:

```bash
git push heroku master
```

⏰ Wait 2-3 minutes while Heroku builds and deploys...

✅ **DEPLOYED!**

### **2.8 Get Your API URL**

Your API is now live at:
```
https://your-app-name.herokuapp.com
```

**Find your URL:**
```bash
heroku info
```

Look for **"Web URL"**

Or check your Heroku dashboard: https://dashboard.heroku.com/apps

✅ **COPY YOUR API URL!**

---

## 🧪 **STEP 3: TEST YOUR API (5 minutes)**

### **3.1 Test Health Endpoint**

Open browser and go to:
```
https://your-app-name.herokuapp.com/api/health
```

You should see:
```json
{
  "status": "ok",
  "message": "CarEase API is running",
  "timestamp": "2025-12-23T07:20:00.000Z"
}
```

✅ **IT WORKS!**

### **3.2 Test API Root**

Go to:
```
https://your-app-name.herokuapp.com/
```

You should see all available endpoints.

✅ **BACKEND IS LIVE!**

### **3.3 View Logs**

If anything doesn't work:

```bash
heroku logs --tail
```

This shows real-time logs.

---

## 🔗 **STEP 4: CONNECT FRONTEND TO BACKEND**

Now update your frontend to call the Heroku API.

### **4.1 Update Frontend HTML**

In your `index.html`, add at the top of `<script>` section:

```javascript
// API Configuration
const API_URL = 'https://your-app-name.herokuapp.com/api';

// API Helper
async function apiCall(endpoint, method = 'GET', data = null) {
  const options = {
    method,
    headers: { 'Content-Type': 'application/json' },
  };
  
  // Add auth token if exists
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
    
    if (!response.ok) {
      throw new Error(result.error || 'API request failed');
    }
    
    return result;
  } catch (error) {
    console.error('API Error:', error);
    throw error;
  }
}

// Registration Handler
async function handleRegister(event) {
  event.preventDefault();
  
  const formData = {
    first_name: document.getElementById('firstName').value,
    last_name: document.getElementById('lastName').value,
    email: document.getElementById('email').value,
    phone: document.getElementById('phone').value,
    password: document.getElementById('password').value,
    is_student: document.getElementById('studentCheck').checked,
    student_institution: document.getElementById('university')?.value,
    student_email: document.getElementById('studentEmail')?.value,
    user_type: 'customer'
  };
  
  try {
    const result = await apiCall('/auth/register', 'POST', formData);
    
    if (result.token) {
      localStorage.setItem('token', result.token);
      localStorage.setItem('user', JSON.stringify(result.user));
      alert('Registration successful!');
      showPage('dashboard');
    }
  } catch (error) {
    alert('Registration failed: ' + error.message);
  }
}

// Login Handler
async function handleLogin(event) {
  event.preventDefault();
  
  const email = document.getElementById('loginEmail').value;
  const password = document.getElementById('loginPassword').value;
  
  try {
    const result = await apiCall('/auth/login', 'POST', { email, password });
    
    if (result.token) {
      localStorage.setItem('token', result.token);
      localStorage.setItem('user', JSON.stringify(result.user));
      alert('Login successful!');
      showPage('dashboard');
    }
  } catch (error) {
    alert('Login failed: ' + error.message);
  }
}

// Load User Bookings
async function loadBookings() {
  try {
    const bookings = await apiCall('/bookings');
    displayBookings(bookings);
  } catch (error) {
    console.error('Failed to load bookings:', error);
  }
}

// Load Vehicles
async function loadVehicles() {
  try {
    const vehicles = await apiCall('/vehicles');
    displayVehicles(vehicles);
  } catch (error) {
    console.error('Failed to load vehicles:', error);
  }
}
```

### **4.2 Update Form Submissions**

Add event listeners to your forms:

```javascript
// Add to your existing showPage function or window.onload
document.addEventListener('DOMContentLoaded', function() {
  // Register form
  const registerForm = document.getElementById('registerForm');
  if (registerForm) {
    registerForm.addEventListener('submit', handleRegister);
  }
  
  // Login form
  const loginForm = document.getElementById('loginForm');
  if (loginForm) {
    loginForm.addEventListener('submit', handleLogin);
  }
  
  // Load data if logged in
  if (localStorage.getItem('token')) {
    loadBookings();
    loadVehicles();
  }
});
```

### **4.3 Add IDs to Forms**

Make sure your forms have the right IDs and input IDs:

```html
<!-- Register Form -->
<form id="registerForm" class="space-y-6">
  <input type="text" id="firstName" placeholder="First Name">
  <input type="text" id="lastName" placeholder="Last Name">
  <input type="email" id="email" placeholder="Email">
  <input type="tel" id="phone" placeholder="Phone">
  <input type="password" id="password" placeholder="Password">
  
  <input type="checkbox" id="studentCheck">
  <select id="university">...</select>
  <input type="email" id="studentEmail" placeholder="Student Email">
  
  <button type="submit">Create Account</button>
</form>

<!-- Login Form -->
<form id="loginForm" class="space-y-6">
  <input type="email" id="loginEmail" placeholder="Email">
  <input type="password" id="loginPassword" placeholder="Password">
  <button type="submit">Sign In</button>
</form>
```

### **4.4 Redeploy Frontend**

1. Save updated HTML
2. Go to Netlify
3. Drag and drop updated file
4. ✅ **Frontend connected to backend!**

---

## ✅ **DEPLOYMENT COMPLETE!**

### **What You Have:**

✅ **Database:** Running on Supabase (Sydney)
✅ **Backend API:** Running on Heroku
✅ **Frontend:** Running on Netlify (carease.com.au)
✅ **Full Integration:** Frontend talks to backend

### **Your URLs:**

- **Website:** https://carease.com.au
- **API:** https://your-app.herokuapp.com
- **Health Check:** https://your-app.herokuapp.com/api/health

---

## 🎯 **TEST END-TO-END:**

1. **Go to your website:** https://carease.com.au
2. **Click "Join Now"** → Register as renter
3. **Fill in the form** (use real email)
4. **Check "I'm a student"**
5. **Submit**
6. ✅ **You should be registered and logged in!**
7. **Check browser console** (F12) - should see API calls
8. **Check Heroku logs:** `heroku logs --tail`

---

## 💰 **COSTS:**

- **Heroku Eco Plan:** $5/month (includes 1000 dyno hours)
- **Supabase Free:** $0/month
- **Netlify:** $0/month
- **Total:** **$5/month** 💵

**Note:** Heroku removed their free tier in 2022, but $5/month is still very affordable!

---

## 🛠️ **USEFUL HEROKU COMMANDS:**

```bash
# View logs
heroku logs --tail

# Check app info
heroku info

# Restart app
heroku restart

# Run database migration
heroku run bash
# Then: node migrate.js

# Check environment variables
heroku config

# Set new environment variable
heroku config:set KEY=value

# Open app in browser
heroku open

# Scale dynos (if needed)
heroku ps:scale web=1
```

---

## 🚨 **TROUBLESHOOTING:**

### **Issue: App crashes on startup**
```bash
heroku logs --tail
```
Look for error messages. Common issues:
- Missing environment variables
- Database connection failed
- Port binding error

**Fix:**
- Check all config vars are set: `heroku config`
- Verify DATABASE_URL is correct
- Make sure `Procfile` exists

### **Issue: Can't connect to database**
- Verify Supabase connection string
- Check if Supabase project is running
- Test connection in Supabase SQL editor

### **Issue: CORS errors**
- Make sure FRONTEND_URL is set correctly
- Check CORS settings in server.js
- Verify Heroku URL is correct

### **Issue: "Application error"**
```bash
heroku logs --tail
```
- Read error message
- Often missing dependencies or env vars

---

## 📱 **WHAT WORKS NOW:**

1. ✅ **User Registration** → Saves to database
2. ✅ **Login** → Returns JWT token
3. ✅ **Student Verification** → 50% discount flag
4. ✅ **Car Listings** → CRUD operations
5. ✅ **Bookings** → With automatic discount
6. ✅ **Reviews** → Rate cars
7. ✅ **User Dashboard** → See bookings

---

## 🎉 **NEXT STEPS:**

### **Immediate:**
1. Test registration on your site
2. Create a test car listing
3. Make a test booking
4. Verify everything saves

### **Soon:**
1. Add Stripe keys for payments
2. Set up email notifications
3. Add car image uploads
4. Deploy to production domain

### **Later:**
1. Add monitoring (New Relic, Sentry)
2. Set up CI/CD pipeline
3. Add automated testing
4. Scale as needed

---

## 🆘 **NEED HELP?**

**Common Commands:**

```bash
# If deployment fails
heroku logs --tail

# If database issues
heroku config:get DATABASE_URL

# If app won't start
heroku ps

# If need to rebuild
git commit --allow-empty -m "Rebuild"
git push heroku main

# Access Heroku bash
heroku run bash
```

---

## 🎊 **CONGRATULATIONS!**

**You now have:**
- ✅ Live database (Supabase)
- ✅ Live API (Heroku)
- ✅ Live website (Netlify)
- ✅ Full integration
- ✅ **WORKING CAR RENTAL PLATFORM!**

**Total deployment time:** ~30 minutes
**Total monthly cost:** $5

**YOU DID IT!** 🚀💪🎉

---

## 📚 **API DOCUMENTATION:**

Your API is now live with these endpoints:

### **Base URL:** `https://your-app.herokuapp.com/api`

**Auth:**
- POST `/auth/register` - Register user
- POST `/auth/login` - Login user
- GET `/auth/me` - Get current user (requires auth)

**Vehicles:**
- GET `/vehicles` - List all cars
- GET `/vehicles/:id` - Get car details
- POST `/vehicles` - Create car (requires auth)
- PUT `/vehicles/:id` - Update car
- DELETE `/vehicles/:id` - Delete car

**Bookings:**
- GET `/bookings` - Get user bookings (requires auth)
- POST `/bookings` - Create booking (requires auth)
- GET `/bookings/:id` - Get booking details
- PUT `/bookings/:id/status` - Update status

**Reviews:**
- GET `/reviews/vehicle/:id` - Get car reviews
- POST `/reviews` - Create review (requires auth)

**Users:**
- GET `/users/profile` - Get profile (requires auth)
- PUT `/users/profile` - Update profile (requires auth)

Test these with Postman or your frontend!

---

**Ready to deploy? Follow this guide step by step!** 🚀
