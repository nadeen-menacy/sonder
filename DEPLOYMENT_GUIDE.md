# 🚀 Deployment Guide

Follow these steps to deploy your chat application.

---

## Part 1: Setup Accounts & Get Credentials

### 1. MongoDB Atlas (Database)

- Go to [mongodb.com/cloud/atlas](https://www.mongodb.com/cloud/atlas)
- Create free account → Create cluster
- Click "Connect" → Create database user
- Add IP: `0.0.0.0/0` (allow all)
- Copy connection string: `mongodb+srv://username:password@cluster.mongodb.net/...`

### 2. Cloudinary (Image Uploads)

- Go to [cloudinary.com](https://cloudinary.com)
- Create free account
- From dashboard, copy:
  - **Cloud Name**
  - **API Key**
  - **API Secret**

### 3. Generate JWT Secret

Open terminal and run:

```bash
node -e "console.log(require('crypto').randomBytes(32).toString('base64'))"
```

Save the output - this is your JWT secret.

### 4. Push Code to GitHub

- Create a new GitHub repository
- Push this code to your repository

---

## Part 2: Deploy Backend on Render

### Step 1: Create Web Service

## Part 2: Deploy Backend on Render

### Step 1: Create Web Service

1. Go to [render.com](https://render.com) and sign in with GitHub
2. Click **"New +"** → **"Web Service"**
3. Find and select your GitHub repository
4. Fill in the settings:

**Basic Settings:**

- **Name:** `chatapp-backend` (or any name you want)
- **Region:** Select closest to you (e.g., Oregon, Frankfurt)
- **Branch:** `main`
- **Root Directory:** `backend`
- **Runtime:** `Node`

**Build & Deploy:**

- **Build Command:** `npm install`
- **Start Command:** `npm start`

**Plan:**

- Select **"Free"**

### Step 2: Add Environment Variables

Scroll down to **"Environment Variables"** section and click **"Add Environment Variable"**.

Add these **7 variables** one by one:

| Key                     | Value                                |
| ----------------------- | ------------------------------------ |
| `MONGODB_URI`           | Paste your MongoDB connection string |
| `JWT_SECRET`            | Paste the secret you generated       |
| `NODE_ENV`              | `production`                         |
| `CLOUDINARY_CLOUD_NAME` | Your Cloudinary cloud name           |
| `CLOUDINARY_API_KEY`    | Your Cloudinary API key              |
| `CLOUDINARY_API_SECRET` | Your Cloudinary API secret           |
| `PORT`                  | `5001`                               |

### Step 3: Deploy

1. Click **"Create Web Service"** at the bottom
2. Wait 3-5 minutes for deployment to complete
3. Once deployed, you'll see a URL like: `https://chatapp-backend-xyz.onrender.com`
4. **IMPORTANT:** Copy this URL - you need it for the frontend!

### Step 4: Test Backend

Visit: `https://your-backend-url.onrender.com/health`

You should see: `{"status":"ok","message":"Backend API is running"}`

---

## Part 3: Deploy Frontend on Vercel

### Step 1: Update Backend URL in Code

1. Open the file `vercel.json` in your code editor
2. Find these two lines:
   ```json
   "destination": "https://YOUR-BACKEND-URL.onrender.com/api/:path*"
   ```
   and
   ```json
   "destination": "https://YOUR-BACKEND-URL.onrender.com/socket.io/:path*"
   ```
3. Replace `YOUR-BACKEND-URL.onrender.com` with your actual Render backend URL
4. Save the file
5. Commit and push to GitHub:
   ```bash
   git add vercel.json
   git commit -m "Add backend URL"
   git push
   ```

### Step 2: Deploy on Vercel

1. Go to [vercel.com](https://vercel.com) and sign in with GitHub
2. Click **"Add New"** → **"Project"**
3. Find and import your GitHub repository
4. Configure the project:

**Project Settings:**

- **Framework Preset:** `Vite`
- **Root Directory:** Leave empty (just `./`)
- **Build Command:** Leave empty
- **Output Directory:** `frontend/dist`
- **Install Command:** Leave empty

**Environment Variables:**

- **Skip this** - No environment variables needed!

5. Click **"Deploy"**
6. Wait 2-3 minutes
7. You'll get a URL like: `https://your-app.vercel.app`

---

## ✅ You're Done!

**Your app is live!**

- Frontend: `https://your-app.vercel.app` (share this with users)
- Backend: `https://your-backend.onrender.com` (for API calls)

### Test Your App:

1. Visit your Vercel URL
2. Sign up with a new account
3. Upload a profile picture
4. Try messaging features
5. **Test on your phone** to make sure login/cookies work!

---

## 🐛 If Something's Not Working

**Backend not responding:**

- Check Render logs for errors
- Verify all 7 environment variables are set correctly
- Make sure MongoDB connection string is correct

**Frontend can't connect to backend:**

- Check that `vercel.json` has the correct backend URL
- Make sure you pushed the changes to GitHub
- Redeploy on Vercel if needed

**Can't login/signup:**

- Check JWT_SECRET is set in Render
- Check MongoDB is accessible (IP whitelist = 0.0.0.0/0)

**Images not uploading:**

- Verify Cloudinary credentials in Render environment variables

---

That's it! Your chat app should now be working. 🎉
