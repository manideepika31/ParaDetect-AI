# Deployment Guide

## Important: This is a Full-Stack Application

This project requires **both** a backend server (Python/FastAPI) and a frontend (React). You cannot deploy it as a simple static website on GitHub Pages alone.

## Deployment Options

### Option 1: Separate Deployments (Recommended)

#### Frontend → Vercel/Netlify (Free)
#### Backend → Render/Railway/Heroku (Free tier available)

---

## Frontend Deployment

### Deploy to Vercel (Recommended - Free)

1. **Push your code to GitHub**

2. **Go to [vercel.com](https://vercel.com)**
   - Sign in with GitHub
   - Click "New Project"
   - Import your repository

3. **Configure build settings:**
   - Framework Preset: `Vite`
   - Root Directory: `frontend`
   - Build Command: `npm run build`
   - Output Directory: `dist`

4. **Add environment variable:**
   - Name: `VITE_API_URL`
   - Value: Your backend URL (e.g., `https://your-backend.onrender.com`)

5. **Deploy!**

### Deploy to Netlify (Alternative - Free)

1. **Push your code to GitHub**

2. **Go to [netlify.com](https://netlify.com)**
   - Sign in with GitHub
   - Click "New site from Git"
   - Choose your repository

3. **Configure build settings:**
   - Base directory: `frontend`
   - Build command: `npm run build`
   - Publish directory: `frontend/dist`

4. **Add environment variable:**
   - Key: `VITE_API_URL`
   - Value: Your backend URL

5. **Deploy!**

---

## Backend Deployment

### Deploy to Render (Recommended - Free)

1. **Go to [render.com](https://render.com)**
   - Sign in with GitHub
   - Click "New +" → "Web Service"
   - Connect your repository

2. **Configure service:**
   - Name: `paradetect-backend`
   - Root Directory: `backend`
   - Runtime: `Python 3`
   - Build Command: `pip install -r requirements-pytorch.txt`
   - Start Command: `uvicorn app_pytorch:app --host 0.0.0.0 --port $PORT`

3. **Add environment variables:**
   - `PYTHON_VERSION`: `3.11.0`
   - `PORT`: `8000` (Render sets this automatically)

4. **Deploy!**

5. **Copy your backend URL** (e.g., `https://paradetect-backend.onrender.com`)

### Deploy to Railway (Alternative - Free)

1. **Go to [railway.app](https://railway.app)**
   - Sign in with GitHub
   - Click "New Project" → "Deploy from GitHub repo"

2. **Configure:**
   - Root Directory: `backend`
   - Build Command: `pip install -r requirements-pytorch.txt`
   - Start Command: `uvicorn app_pytorch:app --host 0.0.0.0 --port $PORT`

3. **Deploy!**

---

## Docker Deployment (Self-Hosted)

### Backend Docker

Build and run:
```bash
cd backend
docker build -t paradetect-backend .
docker run -p 8000:8000 paradetect-backend
```

### Frontend Build

Build for production:
```bash
cd frontend
npm run build
```

Deploy `dist/` folder to any static hosting

### Environment Variables

Backend:
- `MODEL_PATH`: Path to trained model
- `CORS_ORIGINS`: Allowed frontend origins
- `API_HOST`: Server host
- `API_PORT`: Server port

Frontend:
- `VITE_API_URL`: Backend API URL

### Security Checklist

- [ ] Enable HTTPS
- [ ] Configure proper CORS origins
- [ ] Add rate limiting
- [ ] Implement authentication (if needed)
- [ ] Set up monitoring and logging
- [ ] Configure firewall rules
- [ ] Use environment variables for secrets
- [ ] Enable API key authentication

### Monitoring

Recommended tools:
- Sentry for error tracking
- Prometheus + Grafana for metrics
- CloudWatch/DataDog for logs
- Uptime monitoring (UptimeRobot, Pingdom)

### Scaling

- Use load balancer for multiple backend instances
- Implement caching (Redis)
- Use CDN for frontend assets
- Consider serverless deployment (AWS Lambda, Google Cloud Functions)
