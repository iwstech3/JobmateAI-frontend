# 🔄 GitHub Actions Explained - Frontend

**Location**: `JobmateAI-Frontend/docs/DEPLOYMENT/GITHUB_ACTIONS_EXPLAINED.md`  
**Purpose**: Detailed explanation of CI/CD workflows for frontend  
**Audience**: Team members learning CI/CD  
**Date**: November 30, 2025

---

## 📚 Table of Contents

1. [What is GitHub Actions?](#what-is-github-actions)
2. [How It Works](#how-it-works)
3. [Understanding the Frontend Workflow](#understanding-the-frontend-workflow)
4. [Step-by-Step Breakdown](#step-by-step-breakdown)
5. [How Automatic Deployment Works](#how-automatic-deployment-works)
6. [Troubleshooting](#troubleshooting)

---

## 🎯 What is GitHub Actions?

### Definition

**GitHub Actions** is a built-in automation tool in GitHub that allows you to run scripts automatically when certain events happen in your repository.

### Why Use It for Frontend?

| Benefit | Explanation |
|---------|-------------|
| **Automation** | No manual steps - everything happens automatically |
| **Build Verification** | Tests that the app builds successfully before deploying |
| **Time Saving** | Deploy in seconds instead of minutes |
| **Free** | Included with GitHub - no extra cost |
| **Visibility** | See what's happening in real-time |

### Real-World Analogy

Think of GitHub Actions like a **quality checker**:
- You push code → Checker detects it
- Checker builds the app → Makes sure it compiles
- Checker runs tests → Verifies everything works
- Checker notifies Vercel → Tells Vercel to deploy
- Checker notifies you → Tells you if it worked

**Without GitHub Actions**: You'd have to build and test manually every time.

---

## 🔧 How It Works

### The Flow

```
1. You push code to GitHub
   ↓
2. GitHub detects the push
   ↓
3. GitHub Actions workflow triggers
   ↓
4. Workflow runs on a virtual machine
   ↓
5. Workflow builds the Next.js app
   ↓
6. Workflow notifies Vercel (via webhook)
   ↓
7. Vercel redeploys your application
```

### Key Components

1. **Workflow File** (`.yml` file)
   - **What**: Defines what to do
   - **Where**: `.github/workflows/` folder
   - **Format**: YAML

2. **Trigger** (event that starts workflow)
   - **What**: Push, pull request, etc.
   - **Example**: Push to `main` branch

3. **Job** (set of steps)
   - **What**: Runs on a virtual machine
   - **Example**: "Deploy to Vercel" job

4. **Step** (single action)
   - **What**: One command or action
   - **Example**: "Build project"

---

## 📄 Understanding the Frontend Workflow

### File Location

**Path**: `.github/workflows/deploy-frontend.yml`

**Why this location?**:
- `.github/` = Special folder GitHub recognizes
- `workflows/` = Contains workflow definitions
- `.yml` = YAML file extension

**Important**: GitHub only recognizes workflows in this exact location!

### Key Differences from Backend Workflow

| Aspect | Backend | Frontend |
|--------|---------|----------|
| **Runtime** | Python | Node.js |
| **Build** | Install dependencies | Build Next.js app |
| **Platform** | Render | Vercel |

---

## 🔍 Step-by-Step Breakdown

Let's break down our frontend workflow file:

### 1. Workflow Name

```yaml
name: Deploy Frontend to Vercel
```

**What it is**: The name shown in GitHub Actions tab

**Why it matters**: Helps identify this workflow

---

### 2. Triggers

```yaml
on:
  push:
    branches:
      - main
    paths:
      - 'Frontend/**'
      - '.github/workflows/deploy-frontend.yml'
  workflow_dispatch:
```

**Breaking it down**:

**`on: push:`** = "When code is pushed"

**`branches: - main`** = "Only on the main branch"
- **Why main?**: Production branch

**`paths: - 'Frontend/**'`** = "Only if files in Frontend folder change"
- **Why?**: Don't redeploy if only documentation changes

**`workflow_dispatch:`** = "Allow manual triggering"

---

### 3. Jobs

```yaml
jobs:
  deploy:
    name: Deploy to Vercel
    runs-on: ubuntu-latest
```

**Breaking it down**:

**`jobs:`** = "List of jobs"

**`deploy:`** = "Name of this job"

**`runs-on: ubuntu-latest`** = "Use Ubuntu Linux virtual machine"
- **Why Ubuntu?**: Free, reliable, works for Node.js

---

### 4. Steps

#### Step 1: Checkout Code

```yaml
- name: Checkout code
  uses: actions/checkout@v4
```

**What it does**: Downloads your code from GitHub

**Why needed**: Virtual machine needs your code

---

#### Step 2: Set up Node.js

```yaml
- name: Set up Node.js
  uses: actions/setup-node@v4
  with:
    node-version: '20'
    cache: 'npm'
    cache-dependency-path: Frontend/package-lock.json
```

**What it does**: Installs Node.js 20 on the virtual machine

**Breaking it down**:

**`node-version: '20'`** = "Install Node.js version 20"
- **Why 20?**: Latest LTS (Long Term Support) version
- **Why specific?**: Ensures consistency

**`cache: 'npm'`** = "Cache npm packages"
- **What is caching?**: Store packages for faster future builds
- **Why?**: Speeds up workflow (doesn't re-download packages every time)

**`cache-dependency-path: Frontend/package-lock.json`** = "Where to find dependency list"
- **Why?**: Tells GitHub where package-lock.json is (for caching)

**Without this step**: Can't run Node.js code!

---

#### Step 3: Install Dependencies

```yaml
- name: Install dependencies
  working-directory: ./Frontend
  run: npm ci
```

**What it does**: Installs all npm packages

**Breaking it down**:

**`working-directory: ./Frontend`**:
- **What**: Change to Frontend folder first
- **Why**: Our `package.json` is in Frontend folder

**`npm ci`**:
- **What**: "Clean install" - installs exactly what's in package-lock.json
- **Why `ci` not `install`?**: 
  - `ci` is faster
  - `ci` is more reliable (exact versions)
  - `ci` is better for CI/CD

**Without this step**: Missing packages = app won't build!

---

#### Step 4: Build Project

```yaml
- name: Build project
  working-directory: ./Frontend
  run: npm run build
  env:
    NEXT_PUBLIC_API_URL: ${{ secrets.NEXT_PUBLIC_API_URL || 'http://localhost:8000' }}
```

**What it does**: Builds the Next.js app for production

**Breaking it down**:

**`npm run build`**:
- **What**: Runs the build script from package.json
- **What it does**: 
  - Compiles TypeScript to JavaScript
  - Bundles code
  - Optimizes for production
  - Creates `.next` folder with built app

**`env:`** = "Environment variables for this step"

**`NEXT_PUBLIC_API_URL: ${{ secrets.NEXT_PUBLIC_API_URL || 'http://localhost:8000' }}`**:
- **What**: Sets environment variable for build
- **`${{ secrets.XXX }}`**: Uses GitHub secret (if set)
- **`|| 'http://localhost:8000'`**: Fallback if secret not set
- **Why needed**: Build process might need this variable

**Without this step**: No production build = can't deploy!

---

#### Step 5: Run Tests (Future)

```yaml
- name: Run tests (if available)
  working-directory: ./Frontend
  run: |
    # Uncomment when tests are added
    # npm test || true
    echo "Tests will be added later"
  continue-on-error: true
```

**What it does**: Runs tests (currently placeholder)

**Why commented out**: We don't have tests yet

**Future**: When tests are added, uncomment this

---

#### Step 6: Notify Deployment

```yaml
- name: Notify deployment
  run: |
    echo "✅ Frontend deployment triggered!"
    echo "Vercel will automatically redeploy from GitHub"
    echo "Check Vercel dashboard for deployment status"
```

**What it does**: Prints messages to logs

**Why**: Provides visibility

---

## 🔄 How Automatic Deployment Works

### The Complete Flow

```
┌─────────────────┐
│  You push code  │
│  to GitHub      │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ GitHub detects  │
│ the push        │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ GitHub Actions  │
│ workflow runs    │
│ (builds app)    │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Vercel webhook  │
│ is triggered    │
│ (automatic)     │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Vercel redeploys│
│ your site       │
└─────────────────┘
```

### Vercel's Auto-Deploy

**Important**: Vercel has its own auto-deploy feature!

When you connect a GitHub repository to Vercel:
1. Vercel creates a **webhook**
2. Webhook listens for pushes to GitHub
3. When you push, webhook notifies Vercel
4. Vercel automatically redeploys

**So why GitHub Actions?**

GitHub Actions adds:
- ✅ **Build Verification**: Ensures app builds before deploying
- ✅ **Testing**: Can run tests before deployment
- ✅ **Visibility**: See what's happening in GitHub
- ✅ **Consistency**: Same process every time

**Both work together**:
- GitHub Actions: Verifies and builds
- Vercel Webhook: Actually deploys

---

## 🛠️ Setting Up GitHub Actions

### Step 1: Create the Workflow File

**Location**: `.github/workflows/deploy-frontend.yml`

**How to create**:
1. In your repository, create folder: `.github/workflows/`
2. Create file: `deploy-frontend.yml`
3. Copy the workflow content (already provided)

---

### Step 2: Commit and Push

**Why**: GitHub only runs workflows that are in the repository

**Steps**:
```bash
# Make sure you're on the right branch
git checkout deployment/ci-cd-setup

# Add the workflow file
git add .github/workflows/deploy-frontend.yml

# Commit
git commit -m "Add GitHub Actions workflow for frontend deployment"

# Push
git push origin deployment/ci-cd-setup
```

---

### Step 3: Verify in GitHub

**Steps**:
1. Go to your repository on GitHub
2. Click "Actions" tab
3. You should see "Deploy Frontend to Vercel" workflow listed

**If you see it**: ✅ Workflow is active!

---

### Step 4: Test the Workflow

**Steps**:
1. Make a small change
2. Commit and push to `main` branch
3. Go to "Actions" tab
4. Watch workflow execute

---

## 🐛 Troubleshooting

### Problem: Workflow Not Appearing

**Solutions**:
1. Check file location: `.github/workflows/deploy-frontend.yml`
2. Check file extension: `.yml` (not `.yaml`)
3. Check you committed the file

---

### Problem: Build Fails

**Solutions**:
1. Check logs in Actions tab
2. Common issues:
   - TypeScript errors
   - Missing dependencies
   - Import errors
3. Test build locally first:
   ```bash
   cd Frontend
   npm install
   npm run build
   ```

---

### Problem: Vercel Not Redeploying

**Solutions**:
1. Check Vercel webhook is active
2. Check branch in Vercel settings
3. Manually trigger redeployment

---

## 📊 Understanding Workflow Logs

### How to Read Logs

1. Go to Actions tab
2. Click on a workflow run
3. Click on a job
4. Expand steps to see details

### What to Look For

- **Green checkmark** ✅ = Step succeeded
- **Red X** ❌ = Step failed
- **Yellow circle** 🟡 = Step in progress

---

## 🎓 Best Practices

### 1. Use Specific Versions

```yaml
uses: actions/setup-node@v4  # ✅ Good
uses: actions/setup-node@latest  # ❌ Bad
```

### 2. Cache Dependencies

```yaml
cache: 'npm'  # Speeds up builds
```

### 3. Test Build Locally First

If build fails locally, it will fail in workflow.

### 4. Use Working Directory

```yaml
working-directory: ./Frontend
```

---

## 📚 Additional Resources

- **GitHub Actions Docs**: https://docs.github.com/en/actions
- **Vercel Docs**: https://vercel.com/docs
- **Next.js Docs**: https://nextjs.org/docs

---

## ✅ Checklist

- [ ] Workflow file created in `.github/workflows/`
- [ ] File committed and pushed
- [ ] Workflow visible in Actions tab
- [ ] Workflow triggers on push to main
- [ ] All steps execute successfully
- [ ] Vercel redeploys automatically
- [ ] Logs are readable

---

**Created by**: Miss Winny (Project Mentor)  
**Date**: November 30, 2025  
**Version**: 1.0

