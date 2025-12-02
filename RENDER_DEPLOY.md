# 🚀 Render Deployment Checklist

Follow these steps to deploy CashClash to Render.com

## ✅ Pre-Deployment (Done)
- [x] Code pushed to GitHub
- [x] CORS updated for production
- [x] render.yaml configured

## 📋 Deployment Steps

### Step 1: Sign Up for Render
1. Go to https://render.com
2. Click "Get Started"
3. Sign up with GitHub
4. Authorize Render to access your repositories

### Step 2: Deploy from Blueprint (Easiest Method)

1. In Render Dashboard, click **"New +"** → **"Blueprint"**
2. Connect your repository: **YJDevops/CashClash**
3. Render will detect `render.yaml` automatically
4. Click **"Apply"**
5. ⏳ Wait 10-15 minutes for all services to deploy

**That's it!** Render will automatically:
- Create PostgreSQL database
- Deploy backend
- Deploy frontend
- Configure environment variables
- Set up SSL certificates

### Step 3: Get Your URLs

After deployment completes:
- **Frontend**: `https://cashclash.onrender.com`
- **Backend**: `https://cashclash-backend.onrender.com`
- **API Docs**: `https://cashclash-backend.onrender.com/docs`

### Step 4: Create Admin Account

1. Visit your frontend: `https://cashclash.onrender.com`
2. Click "Register"
3. Create your admin account
4. Go to Render Dashboard → cashclash-backend → Shell
5. Run Python shell and promote your user to admin:

```python
from database import SessionLocal
from models import User
db = SessionLocal()
user = db.query(User).filter(User.email == "your-email@example.com").first()
user.role = "admin"
db.commit()
```

Or use the backend API directly via `/docs`

### Step 5: Test Your Deployment

- [ ] Can access frontend
- [ ] Can register/login
- [ ] Can create games (as admin)
- [ ] Can join games
- [ ] Can view store
- [ ] Admin panel works

## ⚠️ Important Notes

### Free Tier Limitations
- Apps sleep after 15 minutes of inactivity
- First request after sleep takes 30-60 seconds (cold start)
- 750 hours/month (enough for 1 app running 24/7)

### Keep Your App Awake (Optional)
Use **UptimeRobot** (free):
1. Sign up at https://uptimerobot.com
2. Add new monitor
3. URL: `https://cashclash.onrender.com`
4. Interval: Every 5 minutes
5. This pings your app to keep it awake

## 🔧 Post-Deployment

### Update Environment Variables (Optional)

Go to Render Dashboard → Service → Environment:

1. **GOOGLE_CLIENT_ID**: Add your Google OAuth client ID
2. **LLM_API_KEY**: Add OpenAI API key (for AI bracket generation)

### Monitor Your App

- **Logs**: Render Dashboard → Service → Logs
- **Metrics**: Render Dashboard → Service → Metrics
- **Events**: Render Dashboard → Service → Events

## 🐛 Troubleshooting

### Backend won't start
- Check logs in Render dashboard
- Verify database is running
- Check environment variables

### Frontend shows blank page
- Check browser console for errors
- Verify `VITE_API_URL` is set correctly
- Check backend is accessible

### Database connection errors
- Verify database service is running
- Check `DATABASE_URL` is set automatically by Render

## 🎉 Success!

Your app is now live at:
**https://cashclash.onrender.com**

Share it with your friends and start gaming! 🎮

---

**Need help?** Check the full DEPLOYMENT_GUIDE.md or Render docs.
