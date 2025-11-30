# 🚀 Complete Deployment Guide - JobMate AI Frontend

**Location**: `JobmateAI-Frontend/docs/DEPLOYMENT/COMPLETE_GUIDE.md`  
**Purpose**: Comprehensive step-by-step guide for deploying the frontend  
**Audience**: Team members and mentor  
**Date**: November 30, 2025

---

## 📋 Table of Contents

1. [Overview](#overview)
2. [Prerequisites](#prerequisites)
3. [Step 1: Frontend Deployment (Vercel)](#step-1-frontend-deployment-vercel)
4. [Step 2: CI/CD Setup (GitHub Actions)](#step-2-cicd-setup-github-actions)
5. [Step 3: Verification](#step-3-verification)
6. [Troubleshooting](#troubleshooting)

---

## 🎯 Overview

### What is Frontend Deployment?

**Frontend deployment** means putting your Next.js/React application online so users can access it through a web browser. Instead of running on `localhost:3000`, it runs on a server and is accessible via a public URL.

### Why Deploy the Frontend?

- ✅ **Accessibility**: Users can access your app from anywhere
- ✅ **Integration**: Frontend can connect to deployed backend API
- ✅ **Testing**: Test your app in a real environment
- ✅ **Demonstration**: Show your work to stakeholders

### What We're Deploying

In this guide, we'll deploy:
1. **Frontend Application** (Next.js on Vercel) - User interface
2. **CI/CD** (GitHub Actions) - Automates future deployments

### Architecture Overview

```
User Browser → Frontend (Vercel) → Backend API (Render) → Database (Supabase)
```

**Why this architecture?**:
- **Separation**: Frontend and backend are separate (can scale independently)
- **Free tier**: Vercel offers free hosting for Next.js
- **Optimization**: Vercel is optimized for Next.js (fast builds, edge caching)

---

## ✅ Prerequisites

### What You Need Before Starting

1. **Backend Deployed** ✅
   - **Why**: Frontend needs backend URL to make API calls
   - **What you need**: Backend URL from Render (e.g., `https://xxx.onrender.com`)
   - **If not done**: Complete backend deployment first!

2. **Vercel Account** (we'll create during deployment)
   - **Why**: Free hosting for Next.js applications
   - **What it provides**: Server, automatic deployments, environment variables

3. **GitHub Repository Access**
   - **Why**: Need to push code and configure workflows
   - **Verify**: Can you push to `iwstech3/JobmateAI-Frontend`?

### Knowledge Requirements

- Basic understanding of:
  - Git (commit, push, branches)
  - Environment variables
  - URLs and HTTP

**Don't worry if you're not an expert** - this guide explains everything!

---

## 🎨 Step 1: Frontend Deployment (Vercel)

### What is Vercel?

**Vercel** is a platform specialized in hosting Next.js and React applications. It's created by the same team that made Next.js, so it's optimized for it.

### Why Vercel for Frontend?

| Feature | Why It Matters |
|---------|----------------|
| **Free Tier** | Unlimited for open source projects |
| **Next.js Optimized** | Built specifically for Next.js (fastest builds) |
| **Auto-Deploy** | Automatically deploys when you push to GitHub |
| **Edge Network** | Content delivered from servers worldwide (fast) |
| **Environment Variables** | Easy configuration management |

### What is Next.js?

**Next.js** is a React framework that provides:
- Server-side rendering
- Automatic code splitting
- Optimized performance
- Easy deployment

Our frontend is built with Next.js, so Vercel is the perfect fit.

### Detailed Steps

#### 1.1 Create Vercel Account

**What we're doing**: Creating an account to use Vercel's hosting services.

**Why**: You need an account to create and manage projects.

**Steps**:
1. **Navigate to**: https://vercel.com
2. **Click**: "Sign Up" (top right button)
3. **Choose**: "Continue with GitHub" (recommended)
   - **Why GitHub?**: Easier integration, no separate password
4. **Authorize**: Click "Authorize Vercel" when prompted
   - **What this does**: Allows Vercel to access your GitHub repos (for deployment)

**Expected Result**: You're logged into Vercel dashboard.

---

#### 1.2 Import the Project

**What we're doing**: Connecting your GitHub repository to Vercel.

**Why**: Vercel needs access to your code to deploy it.

**Steps**:
1. **In dashboard**, click **"Add New..."** (top right)
2. **Select**: "Project"
   - **What this is**: A project represents one deployment
   - **Alternative**: "Team" (for collaboration, not needed now)

3. **Select repository**: `iwstech3/JobmateAI-Frontend`
   - **Why this repo**: Contains our frontend code
   - **If not listed**: Click "Adjust GitHub App Permissions" and authorize

4. **Click**: "Import"
   - **What happens**: Vercel analyzes your repository

**Expected Result**: Repository imported, ready to configure.

---

#### 1.3 Configure the Project

**What we're doing**: Telling Vercel how to build and deploy our application.

**Why**: Vercel needs to know:
- Where the code is
- What framework to use
- How to build it

**Configuration Fields**:

1. **Project Name**: `jobmate-ai-frontend`
   - **What it is**: Name shown in Vercel dashboard
   - **Why**: Helps identify this project
   - **Can be**: Any name you want
   - **Note**: Vercel auto-generates a URL from this name

2. **Framework Preset**: Vercel auto-detects "Next.js" ✅
   - **What it is**: What framework you're using
   - **Why auto-detected**: Vercel reads `package.json` and detects Next.js
   - **If wrong**: Manually select "Next.js"

3. **Root Directory**: `Frontend` ⚠️ **CRITICAL**
   - **What it is**: Where the code is located in the repository
   - **Why**: Our code is in `Frontend/` folder, not root
   - **What happens if wrong**: Vercel won't find the code and deployment fails
   - **How to set**: Click "Edit" next to Root Directory, enter `Frontend`

4. **Build Command**: Leave default (usually `npm run build`)
   - **What it is**: Command to build the application
   - **Why default works**: Vercel detects it from `package.json`
   - **What it does**: Compiles Next.js app for production

5. **Output Directory**: Leave default (usually `.next`)
   - **What it is**: Where build output goes
   - **Why default works**: Next.js standard output directory

6. **Install Command**: Leave default (usually `npm install`)
   - **What it is**: Command to install dependencies
   - **Why default works**: Standard npm command

**Expected Result**: All fields configured, ready to add environment variables.

---

#### 1.4 Configure Environment Variables

**What we're doing**: Setting the backend API URL so frontend can make API calls.

**Why**: The frontend needs to know where the backend is located.

**What are Environment Variables?**

Environment variables are key-value pairs stored outside your code. For frontend:
- **Client-side**: Variables prefixed with `NEXT_PUBLIC_` are accessible in browser
- **Server-side**: Other variables are only on server

**Why `NEXT_PUBLIC_` prefix?**

Next.js has two environments:
- **Server**: Runs on Vercel's server (can access all env vars)
- **Browser**: Runs in user's browser (can only access `NEXT_PUBLIC_` vars)

**Variable to Add**:

1. **`NEXT_PUBLIC_API_URL`**
   - **Key**: `NEXT_PUBLIC_API_URL`
   - **Value**: Your backend URL from Render
     ```
     https://jobmate-ai-backend.onrender.com
     ```
   - **What it does**: Tells frontend where to send API requests
   - **Why needed**: Frontend makes API calls to backend (login, fetch jobs, etc.)
   - **Why `NEXT_PUBLIC_`**: Must be accessible in browser (for API calls)
   - **How to add**:
     1. Click "Environment Variables" section
     2. Click "Add Environment Variable"
     3. Key: `NEXT_PUBLIC_API_URL`
     4. Value: Paste your backend URL (from Render)
     5. Click "Add"

**⚠️ IMPORTANT**: 
- Use the exact backend URL (include `https://`)
- No trailing slash (don't add `/` at the end)
- This is the URL users' browsers will call

**Expected Result**: Environment variable added:
- ✅ `NEXT_PUBLIC_API_URL` = `https://your-backend.onrender.com`

---

#### 1.5 Deploy

**What we're doing**: Creating the project and starting the deployment.

**Why**: This tells Vercel to build and deploy our application.

**Steps**:
1. **Review configuration**: Make sure everything is correct
2. **Click**: "Deploy" (button at the bottom)
3. **Wait**: 2-3 minutes for deployment
   - **What's happening**:
     1. Vercel clones your GitHub repo
     2. Installs dependencies (`npm install`)
     3. Builds the application (`npm run build`)
     4. Deploys to Vercel's edge network
   - **You'll see**: Build logs in real-time
   - **Watch for**: 
     - ✅ "Build successful"
     - ✅ "Ready"

**Expected Result**: Deployment in progress, you see build logs.

---

#### 1.6 Verify the Deployment

**What we're doing**: Confirming the frontend is running and accessible.

**Why**: Need to verify everything works before using it.

**Steps**:
1. **Wait for**: "Ready" message
2. **Note the URL**: Something like `https://jobmate-ai-frontend.vercel.app`
   - **What this is**: Public URL where your frontend is accessible
   - **Format**: `https://[project-name].vercel.app`
   - **Note**: You can add a custom domain later

3. **Test the Frontend**:
   - **Open in browser**: `https://your-frontend-url.vercel.app`
   - **Expected**: Homepage loads
   - **If you see homepage**: ✅ Frontend is working!

4. **Test API Connection** (if auth is ready):
   - **Try to login**: Should call backend API
   - **Check browser console**: Look for API calls
   - **If API calls work**: ✅ Frontend connected to backend!

5. **Check Deployment Details**:
   - **In Vercel dashboard**, click on the deployment
   - **See**: Build logs, deployment time, etc.
   - **Why useful**: Debugging if something goes wrong

**Expected Result**: 
- ✅ Frontend URL accessible
- ✅ Homepage loads correctly
- ✅ (If backend ready) Can make API calls

**📝 Save the frontend URL** - this is your live application!

---

## 🔄 Step 2: CI/CD Setup (GitHub Actions)

### What is CI/CD for Frontend?

Same concept as backend:
- **CI (Continuous Integration)**: Automatically test code when pushed
- **CD (Continuous Deployment)**: Automatically deploy when code is merged

### Why GitHub Actions for Frontend?

| Feature | Why It Matters |
|---------|----------------|
| **Free** | Included with GitHub |
| **Automatic** | Runs on every push/merge |
| **Testing** | Can run tests before deployment |
| **Visibility** | See what's happening in GitHub |

### How It Works

```
You push code → GitHub detects change → GitHub Actions runs → Vercel redeploys
```

**Note**: Vercel also has its own auto-deploy from GitHub. GitHub Actions adds:
- Testing before deployment
- Build verification
- Better visibility

### Detailed Steps

#### 2.1 Understand the Workflow File

**What we're doing**: Understanding what the GitHub Actions workflow does.

**File Location**: `.github/workflows/deploy-frontend.yml`

**Why this location?**:
- `.github/` = Special folder GitHub recognizes
- `workflows/` = Contains workflow definitions
- `.yml` = YAML format

**What the workflow does**:
1. Triggers on push to `main` branch
2. Checks out the code
3. Sets up Node.js
4. Installs dependencies
5. Builds the project
6. (Future) Runs tests
7. Notifies that deployment should happen

**Note**: Vercel's webhook handles actual deployment. This workflow verifies everything is ready.

---

#### 2.2 Create the Workflow File

**What we're doing**: Creating the GitHub Actions workflow file.

**Why**: This file tells GitHub what to do when code is pushed.

**Steps**:
1. **In your local repository**, navigate to: `JobmateAI-Frontend/`
2. **Create folder structure**:
   ```
   .github/
     workflows/
       deploy-frontend.yml
   ```
   - **Why this structure**: GitHub looks for workflows in this exact location

3. **Create the file**: `.github/workflows/deploy-frontend.yml`
   - **Content**: (Already created - see the file)
   - **What it contains**: YAML configuration for the workflow

**Expected Result**: Workflow file created in correct location.

---

#### 2.3 Commit and Push the Workflow

**What we're doing**: Adding the workflow to Git so GitHub can use it.

**Why**: GitHub Actions only runs workflows that are in the repository.

**Steps**:
1. **Check current branch**: Should be on `deployment/ci-cd-setup`
   ```bash
   git branch
   ```

2. **Add the file**:
   ```bash
   git add .github/workflows/deploy-frontend.yml
   ```

3. **Commit**:
   ```bash
   git commit -m "Add GitHub Actions workflow for frontend deployment"
   ```

4. **Push**:
   ```bash
   git push origin deployment/ci-cd-setup
   ```

**Expected Result**: Workflow file pushed to GitHub.

---

#### 2.4 Verify Workflow is Active

**What we're doing**: Confirming GitHub recognizes the workflow.

**Why**: Need to verify it's set up correctly.

**Steps**:
1. **Go to GitHub**: Navigate to your repository
2. **Click**: "Actions" tab (top navigation)
3. **You should see**: "Deploy Frontend to Vercel" workflow listed
4. **If you see it**: ✅ Workflow is active!

**Expected Result**: Workflow visible in Actions tab.

---

#### 2.5 Test Automatic Deployment

**What we're doing**: Testing that pushing code triggers automatic deployment.

**Why**: Verify the entire CI/CD pipeline works.

**Steps**:
1. **Make a small change**: 
   - Edit `README.md` (add a line)
   - Or update a comment in code

2. **Commit and push**:
   ```bash
   git add .
   git commit -m "Test automatic deployment"
   git push origin deployment/ci-cd-setup
   ```

3. **Watch GitHub Actions**:
   - Go to "Actions" tab
   - Click on the latest workflow run
   - Watch it execute in real-time

4. **Check Vercel**:
   - Go to Vercel dashboard
   - Check if new deployment started
   - (Vercel auto-deploys on push to connected branch)

**Expected Result**: 
- ✅ GitHub Actions workflow runs
- ✅ Vercel starts new deployment
- ✅ Everything works automatically!

---

## ✅ Step 3: Verification

### What We're Verifying

1. **Frontend**: Running and accessible
2. **Backend Connection**: Frontend can call backend API
3. **CI/CD**: Automatic deployment works
4. **Integration**: Everything connected correctly

### Verification Checklist

- [ ] **Frontend Accessible**:
  - Homepage loads: `https://your-frontend.vercel.app`
  - No errors in browser console
  - Pages navigate correctly

- [ ] **Backend Connection**:
  - Environment variable `NEXT_PUBLIC_API_URL` is set
  - Frontend can make API calls (check browser Network tab)
  - API calls go to correct backend URL

- [ ] **Environment Variables**:
  - `NEXT_PUBLIC_API_URL` set in Vercel
  - Value is correct (backend URL)
  - No missing variables

- [ ] **GitHub Actions**:
  - Workflow file exists
  - Workflow runs on push
  - No errors in workflow logs

- [ ] **Automatic Deployment**:
  - Push to main triggers deployment
  - Vercel redeploys automatically
  - New code is live

---

## 🆘 Troubleshooting

### Problem: Frontend won't build on Vercel

**Symptoms**: 
- Build fails
- Deployment shows "Build Error"
- Logs show errors

**Solutions**:
1. **Check Root Directory**:
   - Must be `Frontend` (not root)
   - Verify in Vercel project settings

2. **Check Build Command**:
   - Should be: `npm run build` (or auto-detected)
   - Verify in `package.json`

3. **Check Logs**:
   - Go to Vercel → Deployments → Click failed deployment → View logs
   - Look for error messages
   - Common issues:
     - Missing dependencies
     - TypeScript errors
     - Import errors

4. **Test Build Locally**:
   ```bash
   cd Frontend
   npm install
   npm run build
   ```
   - If it fails locally, it will fail on Vercel
   - Fix errors locally first

---

### Problem: Frontend can't connect to backend

**Symptoms**:
- Frontend loads but API calls fail
- Browser console shows CORS errors
- Network tab shows failed requests

**Solutions**:
1. **Verify Environment Variable**:
   - Check `NEXT_PUBLIC_API_URL` is set in Vercel
   - Verify URL is correct (include `https://`)
   - No trailing slash

2. **Test Backend URL**:
   - Open backend URL in browser: `https://your-backend.onrender.com`
   - Should see API response
   - If not, backend might be down

3. **Check CORS**:
   - Backend must allow requests from Vercel domain
   - Check backend CORS settings
   - Should allow: `https://your-frontend.vercel.app`

4. **Check Browser Console**:
   - Open browser DevTools (F12)
   - Go to Console tab
   - Look for error messages
   - Go to Network tab
   - See if API requests are being made

---

### Problem: Environment variable not working

**Symptoms**:
- `NEXT_PUBLIC_API_URL` is set but frontend uses wrong URL
- API calls go to `localhost` instead of production

**Solutions**:
1. **Check Prefix**:
   - Must be `NEXT_PUBLIC_API_URL` (not `API_URL`)
   - `NEXT_PUBLIC_` is required for browser access

2. **Redeploy After Change**:
   - Changing env vars requires redeployment
   - Vercel should auto-redeploy
   - Or manually trigger redeployment

3. **Check Code Usage**:
   - In code, use: `process.env.NEXT_PUBLIC_API_URL`
   - Not: `process.env.API_URL` (won't work in browser)

4. **Verify in Build**:
   - Check Vercel build logs
   - Should show env var is available
   - If not, check Vercel settings

---

### Problem: GitHub Actions not working

**Symptoms**:
- Workflow doesn't appear in Actions tab
- No workflow runs when pushing

**Solutions**:
1. **Check File Location**:
   - Must be: `.github/workflows/deploy-frontend.yml`
   - Not: `github/workflows/` (missing dot)
   - Not: `.github/workflow/` (wrong folder name)

2. **Check YAML Syntax**:
   - Use online YAML validator
   - Common errors: indentation, missing colons

3. **Check Branch**:
   - Workflow might only trigger on `main`
   - Check `on:` section in workflow file

4. **Check Repository Settings**:
   - Go to Settings → Actions
   - Verify Actions are enabled

---

### Problem: Vercel not auto-deploying

**Symptoms**:
- Push to GitHub but Vercel doesn't redeploy

**Solutions**:
1. **Check Webhook**:
   - Go to Vercel → Project Settings → Git
   - Verify GitHub integration is connected
   - Check webhook is active

2. **Check Branch**:
   - Vercel might be watching different branch
   - Verify branch in Vercel settings

3. **Manually Trigger**:
   - In Vercel, click "Redeploy"
   - This redeploys immediately

---

## 📚 Additional Resources

### Documentation Links

- **Vercel Docs**: https://vercel.com/docs
- **Next.js Docs**: https://nextjs.org/docs
- **GitHub Actions Docs**: https://docs.github.com/en/actions

### Support

- **Vercel Support**: https://vercel.com/support
- **GitHub Support**: https://support.github.com

---

## 🎯 Next Steps

After successful deployment:

1. **Test All Pages**:
   - Navigate through all pages
   - Test all functionality
   - Verify API calls work

2. **Monitor Performance**:
   - Check Vercel analytics
   - Monitor build times
   - Watch for errors

3. **Set Up Custom Domain** (optional):
   - Add custom domain in Vercel
   - Configure DNS
   - Update environment variables if needed

4. **Optimize**:
   - Check Lighthouse scores
   - Optimize images
   - Improve performance

---

**Created by**: Miss Winny (Project Mentor)  
**Date**: November 30, 2025  
**Version**: 1.0

