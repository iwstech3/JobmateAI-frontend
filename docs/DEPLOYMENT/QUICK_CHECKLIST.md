# ✅ Quick Deployment Checklist - Frontend

**Location**: `JobmateAI-Frontend/docs/DEPLOYMENT/QUICK_CHECKLIST.md`  
**Purpose**: Fast reference checklist for frontend deployment  
**Audience**: Quick reference during deployment  
**Date**: November 30, 2025

---

## 🎯 Pre-Deployment

- [ ] Backend is deployed (have backend URL ready)
- [ ] Vercel account created
- [ ] GitHub repository access confirmed
- [ ] All team members have access

---

## 📋 Step 1: Frontend (Vercel)

- [ ] Account created at https://vercel.com
- [ ] Project imported: `iwstech3/JobmateAI-Frontend`
- [ ] Branch selected: `main`

**Configuration**:
- [ ] Project Name: `jobmate-ai-frontend`
- [ ] Framework: Auto-detected "Next.js" ✅
- [ ] Root Directory: `Frontend` ⚠️
- [ ] Build Command: Auto-detected `npm run build` ✅
- [ ] Output Directory: Auto-detected `.next` ✅

**Environment Variables**:
- [ ] `NEXT_PUBLIC_API_URL` = (Backend URL from Render)

**Deployment**:
- [ ] Project created
- [ ] Deployment completed (2-3 min wait)
- [ ] Frontend URL received: `https://xxx.vercel.app`
- [ ] Homepage tested: Loads correctly
- [ ] (If backend ready) API connection tested: Can make API calls

**Time**: ~20 minutes

---

## 📋 Step 2: CI/CD (GitHub Actions)

- [ ] Workflow file created: `.github/workflows/deploy-frontend.yml`
- [ ] File committed to repository
- [ ] File pushed to branch: `deployment/ci-cd-setup`
- [ ] Workflow visible in Actions tab
- [ ] Test: Small change pushed to main
- [ ] Workflow runs successfully
- [ ] Vercel auto-redeploys (verified)

**Time**: ~10 minutes

---

## 📋 Step 3: Verification

- [ ] Frontend accessible: `https://xxx.vercel.app`
- [ ] Homepage loads correctly
- [ ] No errors in browser console
- [ ] (If backend ready) API calls work
- [ ] Environment variable set correctly
- [ ] GitHub Actions workflow runs on push

**Time**: ~5 minutes

---

## 📝 Important Information to Document

After deployment, save these:

### URLs:
- **Frontend**: `https://xxx.vercel.app`
- **Backend**: `https://xxx.onrender.com` (from backend deployment)
- **Vercel Dashboard**: https://vercel.com/dashboard

### Environment Variables (Vercel):
- `NEXT_PUBLIC_API_URL` = `https://xxx.onrender.com`

---

## 🆘 Quick Troubleshooting

### Frontend won't build?
1. Check Root Directory = `Frontend`
2. Check build logs in Vercel
3. Test build locally first

### Can't connect to backend?
1. Verify `NEXT_PUBLIC_API_URL` is set
2. Check backend URL is correct
3. Check CORS in backend

### GitHub Actions not working?
1. Check file is in `.github/workflows/`
2. Check YAML syntax
3. Verify webhook enabled in Vercel

---

## ⏱️ Total Time Estimate

- **Frontend Deployment**: 20 minutes
- **CI/CD Setup**: 10 minutes
- **Verification**: 5 minutes
- **Total**: ~35 minutes

---

**Created by**: Miss Winny (Project Mentor)  
**Date**: November 30, 2025

