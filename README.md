# 🚀 Slack Connect - Message Scheduling App

Slack Connect is a full-stack TypeScript application that allows you to connect your Slack workspace, send immediate messages, and schedule messages for future delivery.

---

##### 📝 API Endpoints Messages to any channel or DM
- ⏰ **Schedule Messages** for future delivery
- 📋 **Manage Scheduled Messages** (view, cancel)
- 🔄 **Token Refresh** for continuous authentication
- 🛡️ **Security** with rate limiting and HTTPS

##### 🛠️ Tech Stack

- **Frontend**: React, TypeScript, Vite, Tailwind CSS
- **Backend**: Node.js, Express, TypeScript
- **Database**: SQLite
- **Authentication**: Slack OAuth 2.0
- **Scheduling**: Node-cron

##### 📋 Prerequisites

- Node.js (v16 or higher)
- npm or yarn
- A Slack workspace (free tier works)

##### 🎯 Complete Setup Guide

###### Step 1: Clone and Install

```bash
# Clone the repository
git clone <[your-repo-url](https://github.com/Shubh6665/SlackConnectApp)>
cd SlackConnect

# Install backend dependencies
cd backend
npm install

# Install frontend dependencies
cd ../frontend
npm install
cd ..
```

###### Step 2: Create Slack App

########## 2.1 Go to Slack API Dashboard

1. Visit https://api.slack.com/apps

2. Click **"Create New App"**
3. Choose **"From scratch"**
4. Enter App Name: `"Slack Connect"` (or any name you like)
5. Select your Slack workspace
6. Click **"Create App"**

########## 2.2 Configure OAuth & Permissions

1. In your app dashboard, go to **"OAuth & Permissions"** (left sidebar)
2. Scroll down to **"Scopes"** section
3. Under **"Bot Token Scopes"**, click **"Add an OAuth Scope"**
4. Add these scopes:
   - `channels:read` - View basic information about public channels
   - `chat:write` - Send messages as your app
   - `users:read` - View people in workspace
   - `groups:read` - View basic information about private channels
   - `im:read` - View basic information about direct messages
   - `mpim:read` - View basic information about group direct messages

########## 2.3 Set Redirect URLs

1. Still in **"OAuth & Permissions"**
2. Scroll up to **"Redirect URLs"**
3. Click **"Add New Redirect URL"**

4. Enter: `https://localhost:3001/api/auth/slack/callback`

5. Click **"Add"**
6. Click **"Save URLs"**

########## 2.4 Get App Credentials

1. Go to **"Basic Information"** (left sidebar)
2. Scroll down to **"App Credentials"**
3. Copy these values:
   - **App ID**
   - **Client ID**
   - **Client Secret**
   - **Signing Secret**

###### Step 3: Environment Configuration

########## 3.1 Backend Environment

```bash
cd backend
cp .env.example .env
```

Edit `backend/.env` with your Slack app credentials:

```bash
# Slack OAuth Configuration
SLACK_CLIENT_ID=your_client_id_here
SLACK_CLIENT_SECRET=your_client_secret_here
SLACK_APP_ID=your_app_id_here
SLACK_STATE_SECRET=random_secret_string_here

# Server Configuration
PORT=3001
NODE_ENV=development

FRONTEND_URL=http://localhost:3000

BACKEND_URL=https://localhost:3001

# Database Configuration
DATABASE_PATH=./slack_connect.db
```

########## 3.2 Frontend Environment

```bash
cd frontend
cp .env.example .env
```

Edit `frontend/.env`:

```bash

VITE_API_BASE_URL=https://localhost:3001/api
```

###### Step 4: Run the Application

########## 4.1 Start Backend (Terminal 1)

```bash
cd backend
npm run dev
```

You should see:

```
HTTPS Server is running on port 3001
🔐 You need to accept the self-signed certificate!
```

########## 4.2 Accept HTTPS Certificate

1. Open browser and go to: `https://localhost:3001/api/health`

2. You'll see a security warning ⚠️
3. Click **"Advanced"** or **"Show Details"**
4. Click **"Proceed to localhost (unsafe)"** or **"Accept the risk"**
5. You should see: `{"status":"OK","timestamp":"..."}`

########## 4.3 Start Frontend (Terminal 2)

```bash
cd frontend
npm run dev
```

You should see:

```
➜  Local:   http://localhost:3000/
```

###### Step 5: Install App to Slack Workspace

########## 5.1 Install to Your Workspace

1. In your Slack app dashboard, go to **"Install App"** (left sidebar)
2. Click **"Install to Workspace"**
3. Review permissions and click **"Allow"**
4. Copy the **"Bot User OAuth Token"** (starts with `xoxb-`)
5. You can also find this in **"OAuth & Permissions"** section

########## 5.2 Test the Connection

1. Open http://localhost:3000 in your browser

2. Click **"Connect to Slack"**
3. You should be redirected to Slack for authorization
4. Click **"Allow"** to authorize the app
5. You should be redirected back to the app dashboard

##### 🎉 Usage

###### Send Immediate Message

1. Go to Dashboard
2. Select a channel or DM
3. Type your message
4. Click **"Send Now"**

###### Schedule Message

1. Go to Dashboard
2. Select a channel or DM
3. Type your message
4. Set date and time
5. Click **"Schedule Message"**

###### Manage Scheduled Messages

1. Go to **"Scheduled Messages"** page
2. View all your scheduled messages
3. Cancel any pending message

##### 🔧 Troubleshooting

###### Common Issues

########## 1. "redirect_uri did not match" Error

- Make sure your Slack app redirect URL is: `https://localhost:3001/api/auth/slack/callback`

- Ensure you're using HTTPS (not HTTP)

########## 2. "Certificate Error" in Browser

- You must accept the self-signed certificate first

- Go to `https://localhost:3001/api/health` and accept the security warning

########## 3. "Failed to connect to Slack" Error

- Check if backend is running on port 3001
- Verify your Slack app credentials in `.env`
- Make sure all required OAuth scopes are added

########## 4. Messages Not Sending

- Ensure your bot is added to the channel
- Check if your OAuth token is valid
- Verify the channel ID is correct

###### Development Tips

########## Reset Database

```bash
cd backend
rm slack_connect.db
npm run dev  # Database will be recreated
```

########## Check Logs

Backend logs show important information:

- OAuth flow details
- Database operations
- Message sending status

########## HTTPS in Development

This app uses HTTPS in development because Slack requires it for OAuth redirects. The self-signed certificate is automatically generated.

##### 📝 API Endpoints

###### Authentication

- `GET /api/auth/slack` - Start OAuth flow
- `GET /api/auth/slack/callback` - Handle OAuth callback
- `GET /api/auth/status` - Check auth status

###### Messages

- `GET /api/messages/channels` - Get available channels
- `POST /api/messages/send` - Send immediate message
- `POST /api/messages/schedule` - Schedule message
- `GET /api/messages/scheduled` - Get scheduled messages
- `DELETE /api/messages/scheduled/:id` - Cancel scheduled message

  
 #####  🚀 Production Deployment Guide

This guide will help you deploy your Slack Connect app to production using **Render** (backend) and **Vercel** (frontend).

---

## 📋 Deployment Overview

- **Backend**: Deploy to [Render](https://render.com) (free tier available)
- **Frontend**: Deploy to [Vercel](https://vercel.com) (free tier available)
- **Database**: SQLite (included with backend deployment)

---

## 🛠️ Step 1: Prepare Your Code

### 1.1 Required Files for Deployment

Make sure your project has these essential files:

**Backend Files:**
- `backend/Procfile` - Tells Render how to start your app
- `backend/package.json` - Contains proper scripts and dependencies
- `backend/tsconfig.json` - TypeScript configuration

**Frontend Files:**
- `frontend/vercel.json` - Configures SPA routing for Vercel
- `frontend/public/_redirects` - Backup routing configuration
- `frontend/package.json` - Contains build scripts

**Create missing files if needed:**

```bash
# Create backend/Procfile
echo "web: npm run build && npm start" > backend/Procfile

# Create frontend/vercel.json
cat > frontend/vercel.json << 'EOF'
{
  "rewrites": [
    { "source": "/auth-success", "destination": "/index.html" },
    { "source": "/auth-error", "destination": "/index.html" },
    { "source": "/dashboard", "destination": "/index.html" },
    { "source": "/scheduled", "destination": "/index.html" },
    { "source": "/(.*)", "destination": "/index.html" }
  ]
}
EOF

# Create frontend/public/_redirects
mkdir -p frontend/public
echo "/*    /index.html   200" > frontend/public/_redirects
```

### 1.2 Push to GitHub

Make sure your code is pushed to a GitHub repository:

```bash
# Initialize git (if not done already)
git init
git add .
git commit -m "Initial commit"

# Add your GitHub repo as remote
git remote add origin https://github.com/yourusername/your-repo-name.git
git push -u origin main
```

---

## 🌐 Step 2: Deploy Backend to Render

### 2.1 Create Render Account

1. Go to [render.com](https://render.com)
2. Sign up with GitHub
3. Click **"New +"** → **"Web Service"**

### 2.2 Connect Your Repository

1. Select **"Build and deploy from a Git repository"**
2. Connect your GitHub account
3. Select your repository
4. Click **"Connect"**

### 2.3 Configure Backend Service

**Basic Settings:**
- **Name**: `your-app-name-backend` (choose any unique name)
- **Region**: Select closest to your users
- **Branch**: `main`
- **Root Directory**: `backend`
- **Runtime**: `Node`

**Build Settings:**
- **Build Command**: `npm install && npm run build`
- **Start Command**: `npm start`

### 2.4 Set Environment Variables

In the **Environment** section, add these variables:

```bash
NODE_ENV=production
SLACK_CLIENT_ID=your_slack_client_id
SLACK_CLIENT_SECRET=your_slack_client_secret  
SLACK_APP_ID=your_slack_app_id
SLACK_STATE_SECRET=your_random_secret
FRONTEND_URL=https://your-frontend-domain.vercel.app
BACKEND_URL=https://your-backend-domain.onrender.com
DATABASE_PATH=./slack_connect.db
```

**⚠️ Important**: Replace the values above with:
- Your actual Slack app credentials (from Slack API dashboard)
- Your actual frontend/backend URLs (you'll get these after deployment)
- A random string for `SLACK_STATE_SECRET`

### 2.5 Deploy Backend

1. Click **"Create Web Service"**
2. Wait for deployment to complete (3-5 minutes)
3. Copy your backend URL (e.g., `https://your-app.onrender.com`)

---

## 🎨 Step 3: Deploy Frontend to Vercel

### 3.1 Create Vercel Account

1. Go to [vercel.com](https://vercel.com)
2. Sign up with GitHub
3. Click **"New Project"**

### 3.2 Import Your Repository

1. Select **"Import Git Repository"**
2. Choose your GitHub repository
3. Click **"Import"**

### 3.3 Configure Frontend Project

**Project Settings:**
- **Framework Preset**: `Vite`
- **Root Directory**: `frontend`
- **Build Command**: `npm run build`
- **Output Directory**: `dist`
- **Install Command**: `npm install`

### 3.4 Set Environment Variables

In **Environment Variables**, add:

```bash
VITE_API_BASE_URL=https://your-backend-domain.onrender.com/api
```

Replace `your-backend-domain.onrender.com` with your actual Render backend URL.

### 3.5 Deploy Frontend

1. Click **"Deploy"**
2. Wait for deployment to complete (1-2 minutes)
3. Copy your frontend URL (e.g., `https://your-app.vercel.app`)

---

## 🔧 Step 4: Update Backend Environment

Now that you have your frontend URL, update your backend:

1. Go to your **Render dashboard**
2. Select your backend service
3. Go to **Environment** tab
4. Update `FRONTEND_URL` with your actual Vercel URL:
   ```
   FRONTEND_URL=https://your-app.vercel.app
   ```
5. Click **"Save Changes"** (this will redeploy your backend)

---

## 🔐 Step 5: Update Slack App Settings

### 5.1 Update OAuth Redirect URLs

1. Go to your [Slack API dashboard](https://api.slack.com/apps)
2. Select your app
3. Go to **"OAuth & Permissions"**
4. Under **"Redirect URLs"**, add your production backend URL:
   ```
   https://your-backend-domain.onrender.com/api/auth/slack/callback
   ```
5. Remove any localhost URLs
6. Click **"Save URLs"**

---

## ✅ Step 6: Test Your Deployment

### 6.1 Test Backend Health

Visit: `https://your-backend-domain.onrender.com/api/health`

You should see: `{"status":"OK","timestamp":"..."}`

### 6.2 Test Frontend

Visit: `https://your-app.vercel.app`

You should see your Slack Connect homepage.

### 6.3 Test Full OAuth Flow

1. Click **"Connect to Slack"** on your frontend
2. You should be redirected to Slack for authorization
3. After authorizing, you should be redirected back to your app
4. You should see the dashboard with your Slack workspace info

---

## 🐛 Troubleshooting Deployment

### Backend Issues

**Build Fails:**
- Check if all dependencies are in `package.json` dependencies (not devDependencies)
- Make sure TypeScript builds without errors locally

**502 Bad Gateway:**
- Check Render logs for error messages
- Verify all environment variables are set correctly
- Make sure `DATABASE_PATH=./slack_connect.db`

**OAuth Errors:**
- Verify Slack redirect URLs match your deployed backend URL exactly
- Check that all Slack environment variables are correct

### Frontend Issues

**404 on Routes:**
- Make sure `vercel.json` exists in your frontend folder with SPA routing config
- Verify Vercel root directory is set to `frontend`

**API Errors:**
- Check that `VITE_API_BASE_URL` points to your backend with `/api` suffix
- Verify backend is accessible from frontend domain (CORS)

### Common Environment Variable Issues

**Backend Environment Variables:**
```bash
# ✅ Correct format
FRONTEND_URL=https://your-app.vercel.app
BACKEND_URL=https://your-backend.onrender.com

# ❌ Wrong - don't include trailing slashes
FRONTEND_URL=https://your-app.vercel.app/
BACKEND_URL=https://your-backend.onrender.com/
```

**Frontend Environment Variables:**
```bash
# ✅ Correct format  
VITE_API_BASE_URL=https://your-backend.onrender.com/api

# ❌ Wrong - missing /api or trailing slash
VITE_API_BASE_URL=https://your-backend.onrender.com
VITE_API_BASE_URL=https://your-backend.onrender.com/api/
```

---

## 🔄 Updating Your Deployment

### To Update Backend:
1. Push changes to your GitHub repository
2. Render will automatically redeploy

### To Update Frontend:
1. Push changes to your GitHub repository  
2. Vercel will automatically redeploy

### To Update Environment Variables:
- **Render**: Dashboard → Service → Environment → Update → Save Changes
- **Vercel**: Dashboard → Project → Settings → Environment Variables

---

## 💰 Cost Information

**Free Tier Limits:**
- **Render**: 750 hours/month (enough for most projects)
- **Vercel**: 100GB bandwidth, 6000 minutes build time/month

**Scaling to Paid Plans:**
- **Render**: $7/month for always-on service
- **Vercel**: $20/month for Pro features

---

## 🎯 Quick Deployment Checklist

- [ ] Code pushed to GitHub
- [ ] Backend deployed to Render with correct environment variables
- [ ] Frontend deployed to Vercel with correct API URL
- [ ] Slack app redirect URLs updated to production URLs
- [ ] Backend health endpoint returns 200 OK
- [ ] Frontend loads without errors
- [ ] Full OAuth flow works end-to-end
- [ ] Can send/schedule messages successfully

---

**🎉 Congratulations! Your Slack Connect app is now live in production!**


##### 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if needed
5. Submit a pull request

##### 📄 License

MIT License - feel free to use this project for learning or commercial purposes.

##### 🆘 Need Help?

- Check the troubleshooting section above
- Create an issue on GitHub
- Review Slack API documentation: https://api.slack.com/

---

**Happy Scheduling! 🎯**

#
