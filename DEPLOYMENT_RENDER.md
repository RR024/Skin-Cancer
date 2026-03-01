# Deploying to Render - Complete Guide

## 📋 Prerequisites

1. **GitHub Account** (free) - https://github.com/signup
2. **Render Account** (free) - https://render.com/register
3. **Git Installed** (if not already) - https://git-scm.com/downloads

---

## 📦 Files Required for Deployment

### ✅ Essential Files (Must Have)
```
SKIN_CANCER_PREDICTION/
├── api.py                    # Flask application
├── best_model.h5            # AI model (46 MB)
├── requirements.txt         # Python dependencies
├── render.yaml              # Render configuration
├── templates/
│   └── index.html          # Frontend HTML
└── static/
    ├── style.css           # Styles
    └── script.js           # Frontend JavaScript
```

### ✅ Configuration Files (Already Included)
- `render.yaml` - Render deployment config
- `.gitignore` - Files to exclude from Git
- `requirements.txt` - Python packages

---

## 🚀 Deployment Steps

### Step 1: Initialize Git Repository (if not done)

Open terminal in your project folder:

```bash
cd "d:\My projects\Cancer Prediction\SKIN_CANCER_PREDICTION"
git init
git add .
git commit -m "Initial commit - Skin Cancer Prediction App"
```

### Step 2: Create GitHub Repository

1. Go to https://github.com/new
2. Repository name: `skin-cancer-prediction` (or any name)
3. Set to **Public** (required for free Render tier)
4. **Do NOT** initialize with README (we already have files)
5. Click "Create repository"

### Step 3: Push to GitHub

Copy the commands from GitHub and run:

```bash
# Add remote repository
git remote add origin https://github.com/YOUR_USERNAME/skin-cancer-prediction.git

# Push code
git branch -M main
git push -u origin main
```

### Step 4: Deploy on Render

#### Option A: Using render.yaml (Automatic - Recommended)

1. Go to https://render.com/dashboard
2. Click **"New +"** → **"Blueprint"**
3. Click **"Connect account"** to link GitHub
4. Select your repository: `skin-cancer-prediction`
5. Render will detect `render.yaml` automatically
6. Click **"Apply"**
7. Wait 5-10 minutes for deployment

#### Option B: Manual Setup

1. Go to https://render.com/dashboard
2. Click **"New +"** → **"Web Service"**
3. Connect your GitHub repository
4. Configure:
   - **Name**: `skin-cancer-prediction`
   - **Environment**: `Python 3`
   - **Build Command**: `pip install -r requirements.txt`
   - **Start Command**: `gunicorn api:app`
   - **Instance Type**: `Free`
5. Click **"Create Web Service"**

### Step 5: Monitor Deployment

- Watch the deployment logs
- First deployment takes 5-10 minutes (large model file)
- Status will change to "Live" when ready

### Step 6: Access Your App

Your app will be live at:
```
https://skin-cancer-prediction-xxxx.onrender.com
```
(Replace with your actual URL from Render dashboard)

---

## ⚙️ Important Render Configuration

### Current `render.yaml` Settings:
```yaml
services:
  - type: web
    name: ai-skin-cancer-app
    env: python
    buildCommand: "pip install -r requirements.txt"
    startCommand: "gunicorn api:app"
    envVars:
      - key: PYTHON_VERSION
        value: 3.10.0
```

### Free Tier Limitations:
- ⚠️ **Spins down after 15 minutes of inactivity**
- ⚠️ **First request after sleep takes 30-50 seconds**
- ✅ 512 MB RAM (sufficient for this app)
- ✅ 0.1 CPU (sufficient)

---

## 🔧 Environment Variables (Optional)

If needed, add in Render dashboard under "Environment":

```
PYTHON_VERSION=3.10.0
PORT=5000
TF_ENABLE_ONEDNN_OPTS=0
TF_CPP_MIN_LOG_LEVEL=2
```

---

## 📝 Post-Deployment Checklist

- [ ] App is accessible via Render URL
- [ ] Homepage loads correctly
- [ ] Model health check works: `https://your-app.onrender.com/health`
- [ ] Image upload and prediction works
- [ ] Check logs for any errors

---

## 🐛 Troubleshooting

### Build Fails - "No module named 'tensorflow'"
**Solution**: Check `requirements.txt` has all dependencies

### App Crashes - Memory Error
**Solution**: 
- Free tier has 512 MB RAM
- This app needs ~300-400 MB
- Should work on free tier
- If issues persist, upgrade to paid tier ($7/month)

### Model Not Loading
**Check**:
1. `best_model.h5` is committed to Git
2. File size: ~46 MB (verify it uploaded)
3. Check Render logs for loading errors

### Slow First Load
**Normal**: Free tier apps sleep after 15 minutes. First request wakes it up (30-50 sec)

### Git Push Fails - File Too Large
If GitHub rejects large files:
```bash
# Install Git LFS (Large File Storage)
git lfs install
git lfs track "*.h5"
git add .gitattributes
git commit -m "Track large files with Git LFS"
git push
```

---

## 🔄 Updating Your Deployment

After making changes:

```bash
git add .
git commit -m "Description of changes"
git push
```

Render automatically redeploys on push!

---

## 💰 Costs

- **Free Tier**: $0/month
  - 750 hours/month free
  - Apps spin down after inactivity
  - Perfect for demos and testing

- **Paid Tier**: $7/month
  - Always on (no spin down)
  - Better performance
  - More RAM options

---

## 📊 Monitoring

View in Render Dashboard:
- Deployment logs
- Application logs
- Performance metrics
- Recent deployments

---

## 🔒 Security Notes

1. Never commit sensitive data (API keys, passwords)
2. Use environment variables for secrets
3. This app has no secrets (educational use)
4. Model file is safe to commit

---

## 🎯 Quick Deploy Checklist

```bash
# 1. Initialize Git
git init

# 2. Add files
git add .

# 3. Commit
git commit -m "Initial commit"

# 4. Create GitHub repo and push
git remote add origin https://github.com/YOUR_USERNAME/REPO_NAME.git
git branch -M main
git push -u origin main

# 5. Go to Render.com → New Blueprint → Connect repo
# 6. Done! Wait for deployment
```

---

## 📞 Need Help?

- **Render Docs**: https://render.com/docs
- **Render Community**: https://community.render.com
- **Status Page**: https://status.render.com

---

**🎉 Your AI app will be live on the internet!**
