# 🔧 Fix: GitHub Showing README Instead of Your App

## Why This Happens

GitHub is **not a hosting platform** for full-stack applications. When you push code to GitHub and click the link, it shows your repository (README file), not a running application.

Your ParaDetect AI needs:
1. **Backend Server** - Python/FastAPI running continuously
2. **Frontend Website** - React app for users to visit

GitHub only stores your code, it doesn't run it.

## The Solution

You need to deploy to **actual hosting platforms**:

### ✅ What You Should Do

1. **Keep your code on GitHub** (for version control)
2. **Deploy Backend** to Render.com (free)
3. **Deploy Frontend** to Vercel.com (free)

## Step-by-Step Fix

### 1. Push Your Code to GitHub (You've Done This ✓)

```bash
git add .
git commit -m "Ready for deployment"
git push origin main
```

### 2. Deploy Backend to Render

1. Go to https://render.com
2. Sign in with GitHub
3. Click "New +" → "Web Service"
4. Select your repository
5. Settings:
   - Root Directory: `backend`
   - Build Command: `pip install -r requirements-pytorch.txt`
   - Start Command: `uvicorn app_pytorch:app --host 0.0.0.0 --port $PORT`
6. Click "Create Web Service"
7. **Copy the URL** (e.g., `https://paradetect-backend.onrender.com`)

### 3. Deploy Frontend to Vercel

1. Go to https://vercel.com
2. Sign in with GitHub
3. Click "New Project"
4. Import your repository
5. Settings:
   - Framework: Vite
   - Root Directory: `frontend`
   - Build Command: `npm run build`
   - Output Directory: `dist`
6. Environment Variables:
   - Name: `VITE_API_URL`
   - Value: Your backend URL from step 2
7. Click "Deploy"
8. **This is your live app URL!** (e.g., `https://paradetect-ai.vercel.app`)

### 4. Update Backend CORS

Edit `backend/app_pytorch.py` to allow your Vercel URL:

```python
app.add_middleware(
    CORSMiddleware,
    allow_origins=[
        "http://localhost:5173",
        "https://your-app.vercel.app",  # Add your Vercel URL here
    ],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)
```

Push the changes:
```bash
git add backend/app_pytorch.py
git commit -m "Update CORS for production"
git push origin main
```

Render will automatically redeploy.

## What Each Platform Does

| Platform | Purpose | Cost | What It Hosts |
|----------|---------|------|---------------|
| **GitHub** | Code storage | Free | Your source code (not running) |
| **Render** | Backend hosting | Free | Python API server (running 24/7) |
| **Vercel** | Frontend hosting | Free | React website (what users see) |

## Common Mistakes

### ❌ Mistake 1: Trying to use GitHub Pages
**Problem**: GitHub Pages only hosts static HTML/CSS/JS, not Python servers

**Solution**: Use Render for backend, Vercel for frontend

### ❌ Mistake 2: Only deploying frontend
**Problem**: Frontend needs backend API to work

**Solution**: Deploy both backend AND frontend

### ❌ Mistake 3: Wrong environment variables
**Problem**: Frontend can't find backend

**Solution**: Set `VITE_API_URL` in Vercel to your Render backend URL

## After Deployment

### Your GitHub Repository
- URL: `https://github.com/yourusername/paradetect-ai`
- Shows: Code, README, files
- Purpose: Version control and collaboration

### Your Live Application
- Backend API: `https://paradetect-backend.onrender.com`
- Frontend App: `https://paradetect-ai.vercel.app`
- Purpose: What users actually use

## Testing Your Deployment

1. **Test Backend**:
   ```bash
   curl https://your-backend.onrender.com/health
   ```
   Should return: `{"status":"healthy","model_loaded":true}`

2. **Test Frontend**:
   - Visit your Vercel URL
   - Upload an image
   - Should get prediction results

## Costs

Both platforms are **FREE** for this project:
- Render: 750 hours/month free (enough for 24/7)
- Vercel: Unlimited for personal projects

## Need Help?

See detailed instructions in:
- `DEPLOY_INSTRUCTIONS.md` - Complete deployment guide
- `DEPLOYMENT.md` - Technical deployment details

## Quick Summary

```
GitHub (Code Storage)
    ↓
    ├─→ Render.com (Backend) → https://your-backend.onrender.com
    └─→ Vercel.com (Frontend) → https://your-app.vercel.app ← Share this URL!
```

**Share your Vercel URL with others** - that's your live application! 🚀
