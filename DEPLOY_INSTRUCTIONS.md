# 🚀 How to Deploy ParaDetect AI

## The Problem You're Facing

When you click a GitHub link, it shows the README because GitHub is not a hosting platform for full-stack applications. Your app needs:

1. **Backend Server** - Python/FastAPI running 24/7
2. **Frontend Website** - React app that users visit

## Solution: Use Free Hosting Platforms

### Step 1: Deploy Backend (5 minutes)

**Use Render.com (Free & Easy)**

1. Go to https://render.com
2. Sign up with your GitHub account
3. Click "New +" → "Web Service"
4. Connect your GitHub repository
5. Configure:
   - **Name**: `paradetect-backend`
   - **Root Directory**: `backend`
   - **Environment**: `Python 3`
   - **Build Command**: `pip install -r requirements-pytorch.txt`
   - **Start Command**: `uvicorn app_pytorch:app --host 0.0.0.0 --port $PORT`
6. Click "Create Web Service"
7. Wait 5-10 minutes for deployment
8. **Copy your backend URL** (looks like: `https://paradetect-backend.onrender.com`)

### Step 2: Deploy Frontend (3 minutes)

**Use Vercel.com (Free & Easy)**

1. Go to https://vercel.com
2. Sign up with your GitHub account
3. Click "New Project"
4. Import your repository
5. Configure:
   - **Framework Preset**: `Vite`
   - **Root Directory**: `frontend`
   - **Build Command**: `npm run build`
   - **Output Directory**: `dist`
6. Add Environment Variable:
   - **Name**: `VITE_API_URL`
   - **Value**: Your backend URL from Step 1 (e.g., `https://paradetect-backend.onrender.com`)
7. Click "Deploy"
8. Wait 2-3 minutes
9. **Your app is live!** You'll get a URL like: `https://paradetect-ai.vercel.app`

### Step 3: Update CORS in Backend

After deployment, you need to allow your frontend URL to access the backend:

1. Go to your Render dashboard
2. Click on your backend service
3. Go to "Environment" tab
4. Add environment variable:
   - **Key**: `CORS_ORIGINS`
   - **Value**: Your Vercel URL (e.g., `https://paradetect-ai.vercel.app`)
5. Save and redeploy

Or update `backend/app_pytorch.py` and push to GitHub:

```python
app.add_middleware(
    CORSMiddleware,
    allow_origins=[
        "http://localhost:5173",
        "https://paradetect-ai.vercel.app",  # Add your Vercel URL
        "https://your-custom-domain.com"     # Add any custom domains
    ],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)
```

## Alternative Platforms

### Backend Alternatives:
- **Railway.app** - Similar to Render, very easy
- **Heroku** - Classic option (paid now)
- **PythonAnywhere** - Good for Python apps
- **AWS/Google Cloud** - More complex but scalable

### Frontend Alternatives:
- **Netlify** - Similar to Vercel
- **GitHub Pages** - Only for static sites (won't work for this)
- **Cloudflare Pages** - Fast and free

## Cost

Both Render and Vercel offer **FREE tiers** that are perfect for this project:

- **Render Free**: 750 hours/month (enough for 24/7)
- **Vercel Free**: Unlimited bandwidth for personal projects

## Troubleshooting

### "Failed to analyze image" error after deployment

**Problem**: Frontend can't reach backend

**Solution**:
1. Check backend is running (visit `https://your-backend.onrender.com/health`)
2. Verify CORS settings include your frontend URL
3. Check environment variable `VITE_API_URL` is set correctly in Vercel

### Backend takes long to respond

**Problem**: Render free tier "spins down" after 15 minutes of inactivity

**Solution**:
- First request after inactivity takes 30-60 seconds (this is normal on free tier)
- Upgrade to paid tier ($7/month) for always-on service
- Or use a service like UptimeRobot to ping your backend every 10 minutes

### Model file too large

**Problem**: Git doesn't allow files over 100MB

**Solution**:
1. Add `backend/models/*.pth` to `.gitignore`
2. Train model after deployment using a startup script
3. Or use Git LFS (Large File Storage)

## Quick Commands

### Build frontend locally:
```bash
cd frontend
npm run build
```

### Test backend locally:
```bash
cd backend
python -m uvicorn app_pytorch:app --reload
```

### Check if backend is working:
```bash
curl https://your-backend.onrender.com/health
```

## Summary

1. ✅ Deploy backend to Render (free)
2. ✅ Deploy frontend to Vercel (free)
3. ✅ Connect them with environment variables
4. ✅ Share your Vercel URL with others!

Your app will be live at: `https://your-app-name.vercel.app`

No more README files - a real working application! 🎉
