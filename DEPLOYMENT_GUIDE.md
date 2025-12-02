# CashClash Deployment Guide 🚀

Complete guide to deploy CashClash online with a **FREE domain**.

## 🎯 Recommended Free Hosting Solution

**Render.com** - Best free option for full-stack apps
- ✅ Free PostgreSQL database
- ✅ Free backend hosting
- ✅ Free frontend hosting
- ✅ Free SSL certificate
- ✅ Free subdomain (yourapp.onrender.com)
- ✅ Auto-deploy from GitHub

---

## 📋 Prerequisites

1. GitHub account (✅ Already done!)
2. Render.com account (free)
3. Your CashClash repository on GitHub

---

## 🚀 Step-by-Step Deployment

### Step 1: Sign Up for Render

1. Go to https://render.com
2. Click "Get Started for Free"
3. Sign up with your GitHub account
4. Authorize Render to access your repositories

### Step 2: Create PostgreSQL Database

1. In Render Dashboard, click **"New +"** → **"PostgreSQL"**
2. Configure:
   - **Name**: `cashclash-db`
   - **Database**: `cashclash`
   - **User**: `cashclash_user`
   - **Region**: Choose closest to you
   - **Plan**: **Free**
3. Click **"Create Database"**
4. ⏳ Wait 2-3 minutes for database to be ready
5. **Save the Internal Database URL** (you'll need this)

### Step 3: Deploy Backend (FastAPI)

1. Click **"New +"** → **"Web Service"**
2. Connect your GitHub repository: **YJDevops/CashClash**
3. Configure:
   - **Name**: `cashclash-backend`
   - **Region**: Same as database
   - **Branch**: `main`
   - **Root Directory**: `backend`
   - **Runtime**: `Python 3`
   - **Build Command**: `pip install -r requirements.txt`
   - **Start Command**: `uvicorn main:app --host 0.0.0.0 --port $PORT`
   - **Plan**: **Free**

4. **Add Environment Variables**:
   Click "Advanced" → "Add Environment Variable"
   
   ```
   DATABASE_URL = [Paste Internal Database URL from Step 2]
   SECRET_KEY = [Generate random string, e.g., your-super-secret-key-12345]
   GOOGLE_CLIENT_ID = [Your Google OAuth Client ID - optional for now]
   LLM_API_KEY = [Your OpenAI API key - optional]
   ```

5. Click **"Create Web Service"**
6. ⏳ Wait 5-10 minutes for deployment
7. **Save your backend URL**: `https://cashclash-backend.onrender.com`

### Step 4: Update Backend CORS

Before deploying frontend, we need to update CORS settings:

1. In your local project, edit `backend/main.py`
2. Update the `origins` list:

```python
origins = [
    "http://localhost:3000",
    "http://localhost:5173",
    "https://cashclash.onrender.com",  # Add your frontend URL
    "https://*.onrender.com",  # Allow all Render subdomains
]
```

3. Commit and push:
```bash
git add backend/main.py
git commit -m "Update CORS for production"
git push origin main
```

4. Render will auto-deploy the backend with new settings

### Step 5: Deploy Frontend (React)

1. Click **"New +"** → **"Static Site"**
2. Connect your GitHub repository: **YJDevops/CashClash**
3. Configure:
   - **Name**: `cashclash`
   - **Branch**: `main`
   - **Root Directory**: `frontend`
   - **Build Command**: `npm install && npm run build`
   - **Publish Directory**: `dist`

4. **Add Environment Variable**:
   ```
   VITE_API_URL = https://cashclash-backend.onrender.com
   ```

5. Click **"Create Static Site"**
6. ⏳ Wait 5-10 minutes for deployment

### Step 6: Get Your Free Domain

Your app will be live at:
- **Frontend**: `https://cashclash.onrender.com`
- **Backend API**: `https://cashclash-backend.onrender.com`
- **API Docs**: `https://cashclash-backend.onrender.com/docs`

---

## 🎨 Optional: Custom Free Domain

### Option 1: Freenom (Free .tk, .ml, .ga domains)

1. Go to https://www.freenom.com
2. Search for available domain (e.g., `cashclash.tk`)
3. Register for free (12 months)
4. In Render, go to your frontend service
5. Settings → Custom Domains → Add Custom Domain
6. Follow DNS configuration instructions

### Option 2: Free Subdomains

- **is-a.dev** - Free `.is-a.dev` subdomain for developers
- **js.org** - Free `.js.org` for JavaScript projects
- **eu.org** - Free `.eu.org` domains

---

## 🔧 Post-Deployment Configuration

### 1. Update Frontend API URL

Edit `frontend/src/api.js`:

```javascript
const API_URL = import.meta.env.VITE_API_URL || 'https://cashclash-backend.onrender.com';
```

### 2. Create Admin Account

Access your backend API docs:
`https://cashclash-backend.onrender.com/docs`

Use the `/auth/register` endpoint to create admin account manually, or:

1. SSH into Render backend (if needed)
2. Run database migration to create default admin

### 3. Test Your Deployment

1. Visit `https://cashclash.onrender.com`
2. Register a new account
3. Test game creation (if admin)
4. Test joining games
5. Test store functionality

---

## ⚠️ Important Notes

### Free Tier Limitations

**Render Free Tier**:
- ✅ 750 hours/month (enough for 1 app)
- ⚠️ Apps sleep after 15 min of inactivity
- ⚠️ Cold start takes 30-60 seconds
- ✅ 100GB bandwidth/month
- ✅ PostgreSQL: 1GB storage

**Solutions for Sleep Issue**:
1. Use **UptimeRobot** (free) to ping your app every 5 minutes
2. Upgrade to paid plan ($7/month) for always-on

### Database Backups

Free PostgreSQL on Render:
- ⚠️ No automatic backups
- 💡 Manually export data regularly
- 💡 Consider upgrading for production use

---

## 🔐 Security Checklist

Before going live:

- [ ] Change `SECRET_KEY` to a strong random value
- [ ] Set up Google OAuth properly
- [ ] Enable HTTPS only (Render does this automatically)
- [ ] Review CORS settings
- [ ] Set up environment variables properly
- [ ] Don't commit `.env` files to GitHub
- [ ] Review admin account security

---

## 🐛 Troubleshooting

### Backend won't start
- Check logs in Render dashboard
- Verify `DATABASE_URL` is set correctly
- Ensure all dependencies in `requirements.txt`

### Frontend can't connect to backend
- Check `VITE_API_URL` environment variable
- Verify CORS settings in `backend/main.py`
- Check backend is running (visit `/docs`)

### Database connection errors
- Verify database is running
- Check `DATABASE_URL` format
- Ensure database and backend are in same region

### 500 Errors
- Check backend logs in Render
- Verify all environment variables are set
- Check database migrations ran successfully

---

## 📊 Monitoring Your App

### Free Monitoring Tools

1. **Render Dashboard**
   - View logs
   - Monitor CPU/Memory
   - Check deployment status

2. **UptimeRobot** (https://uptimerobot.com)
   - Monitor uptime
   - Get alerts if app goes down
   - Keep app awake (ping every 5 min)

3. **Google Analytics** (optional)
   - Track user activity
   - Monitor page views
   - Analyze user behavior

---

## 🚀 Alternative Free Hosting Options

### Option 2: Railway.app
- Similar to Render
- $5 free credit/month
- Easy deployment
- PostgreSQL included

### Option 3: Vercel (Frontend) + Railway (Backend)
- **Vercel**: Free frontend hosting
- **Railway**: Free backend + database
- Best performance
- Separate deployments

### Option 4: Fly.io
- Free tier available
- Global deployment
- PostgreSQL included
- More complex setup

---

## 📝 Quick Commands Reference

```bash
# Update and redeploy
git add .
git commit -m "Your changes"
git push origin main

# Check deployment status
# Visit Render dashboard

# View logs
# Render Dashboard → Your Service → Logs

# Rollback deployment
# Render Dashboard → Your Service → Manual Deploy → Select previous commit
```

---

## 🎉 Your App is Live!

Once deployed, share your app:
- **Live URL**: `https://cashclash.onrender.com`
- **API Docs**: `https://cashclash-backend.onrender.com/docs`
- **GitHub**: `https://github.com/YJDevops/CashClash`

**Default Admin Login** (if you set it up):
```
Email: admin@cashclash.com
Password: Admin123!@#
```

---

## 📞 Need Help?

- Render Docs: https://render.com/docs
- Render Community: https://community.render.com
- Your GitHub Issues: https://github.com/YJDevops/CashClash/issues

**Good luck with your deployment! 🚀**
