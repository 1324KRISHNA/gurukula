# 🚀 Vercel Deployment Guide
## Shubham Karoti Maitreyi Gurukula — Child Journey Tracking System

This repository is aligned for **single-click zero-config deployment on [Vercel](https://vercel.com)**.

---

### 🏛️ Architecture on Vercel

```
                        ┌───────────────────────────────────────────────┐
                        │             Vercel Edge Network               │
                        │           https://<your-app>.vercel.app       │
                        └──────────────────────┬────────────────────────┘
                                               │
                      ┌────────────────────────┴────────────────────────┐
                      │                                                 │
          Static Assets & Routes                            API Requests (/api/*, /health)
                      │                                                 │
                      ▼                                                 ▼
          ┌───────────────────────┐                         ┌───────────────────────┐
          │      client/dist      │                         │     api/index.ts      │
          │   React 18 + Vite     │                         │   Serverless Express  │
          │   Single Page App     │                         │   Mongoose Cached DB  │
          └───────────────────────┘                         └───────────┬───────────┘
                                                                        │
                                                                        ▼
                                                            ┌───────────────────────┐
                                                            │     MongoDB Atlas     │
                                                            │    Cloud Database     │
                                                            └───────────────────────┘
```

- **Frontend**: React 18 + Vite + Tailwind CSS compiled to `client/dist`. Vercel automatically serves static assets with optimal caching and rewrites client-side navigation to `/index.html`.
- **Backend API**: The Express application runs inside a serverless function at `/api` (`api/index.ts`), with Mongoose connection reuse and cold-start auto-seeding.
- **Filesystem**: File uploads gracefully fall back to `/tmp/gurukula_uploads` in serverless mode to avoid read-only filesystem errors (`EROFS`).

---

### 🔑 Required Environment Variables

Configure these in your Vercel Project Settings (**Settings → Environment Variables**):

| Variable | Description | Required | Example |
| :--- | :--- | :---: | :--- |
| `MONGODB_URI` | MongoDB Atlas connection string | **YES** | `mongodb+srv://user:pass@cluster0.abcde.mongodb.net/gurukula_db?retryWrites=true&w=majority` |
| `JWT_SECRET` | Secret key for JWT sessions | **YES** | `gurukula-sacred-journey-secure-jwt-secret-2026` |
| `NODE_ENV` | Environment mode | **YES** | `production` |
| `JWT_EXPIRES_IN` | Session duration | No (Default: `7d`) | `7d` |
| `CORS_ORIGIN` | Allowed CORS origins | No | `https://your-custom-domain.org` (Vercel domains auto-allowed) |

> 💡 **MongoDB Atlas Setup Tip**: In your MongoDB Atlas Dashboard, make sure to add `0.0.0.0/0` (Allow Access from Anywhere) under **Network Access** so Vercel's serverless IP addresses can connect.

---

### 📋 Deploying via Vercel Web Dashboard (Recommended)

1. **Push your code to GitHub / GitLab / Bitbucket**:
   ```bash
   git add .
   git commit -m "feat: align repository for Vercel serverless deployment"
   git push origin main
   ```

2. **Import into Vercel**:
   - Go to [vercel.com/new](https://vercel.com/new).
   - Select your Git repository.
   - In **Project Settings**:
     - **Framework Preset**: Vite (or leave default; `vercel.json` automatically takes care of configuration).
     - **Root Directory**: `./` (leave at repository root).
     - **Build and Output Settings**: Pre-configured by `vercel.json`:
       - Install Command: `npm run install:all && npm install`
       - Build Command: `npm run build`
       - Output Directory: `client/dist`
   - Expand **Environment Variables** and add `MONGODB_URI` and `JWT_SECRET`.
   - Click **Deploy**.

---

### 💻 Deploying via Vercel CLI

1. Install Vercel CLI (if not already installed):
   ```bash
   npm install -g vercel
   ```

2. Log in and deploy from project root:
   ```bash
   vercel
   ```

3. Set production environment variables when prompted or run:
   ```bash
   vercel env add MONGODB_URI
   vercel env add JWT_SECRET
   ```

4. Deploy to production:
   ```bash
   vercel --prod
   ```

---

### 🌱 Automatic Zero-Config Database Seeding

When deployed with a fresh MongoDB Atlas database, the application will **automatically seed initial role accounts, sample student profiles, academic terms, and developmental records** on the first request.

#### Ready-to-Use Demo Accounts:

| Role | Email | Password |
| :--- | :--- | :--- |
| **System Administrator** | `admin@gurukula.org` | `GurukulaAdmin2026!` |
| **Admissions Coordinator** | `admissions@gurukula.org` | `GurukulaAdmissions2026!` |
| **Academic Coordinator** | `coordinator@gurukula.org` | `GurukulaCoord2026!` |
| **Teacher / Mentor** | `teacher@gurukula.org` | `GurukulaTeacher2026!` |
| **Management / Trustee** | `trustee@gurukula.org` | `GurukulaTrustee2026!` |

---

### 🔍 Verification & Health Check

After deployment, test the health check endpoint:
```
GET https://<your-app>.vercel.app/health
```
Response:
```json
{
  "status": "UP",
  "service": "Shubham Karoti Maitreyi Gurukula API",
  "timestamp": "2026-11-02T16:30:00.000Z",
  "platform": "Vercel Serverless"
}
```
And access the web application at:
```
https://<your-app>.vercel.app/
```
