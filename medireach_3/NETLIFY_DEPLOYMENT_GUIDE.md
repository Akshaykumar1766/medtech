# MediReach – Netlify Deployment Guide

This guide walks you through deploying **MediReach** to **Netlify** in under 3 minutes, ensuring that all 8 Yelahanka pharmacies, ~50 medicines, demo accounts, and the **Delay Fallback Engine** work with 100% fidelity.

---

## 🚀 Option A: Deploy via GitHub / Git (Recommended)

1. **Push the `medireach_3` folder to a GitHub repository:**
   ```bash
   cd medireach_3
   git init
   git add .
   git commit -m "Initial commit for Netlify deployment"
   git branch -M main
   git remote add origin https://github.com/<your-username>/<your-repo-name>.git
   git push -u origin main
   ```

2. **Connect to Netlify:**
   - Go to [Netlify Dashboard](https://app.netlify.com).
   - Click **"Add new site"** → **"Import an existing project"**.
   - Select **GitHub** and choose your repository.

3. **Verify Build Settings:**
   Netlify will automatically detect settings from [`netlify.toml`](./netlify.toml):
   - **Framework:** `Next.js`
   - **Build command:** `npm run build`
   - **Publish directory:** `.next`
   - **Node version:** `20`

4. **Click "Deploy Site"**:
   - The build command will automatically run:
     1. `prisma generate` (compiles Linux-compatible engine binaries)
     2. `prisma db push` (syncs SQLite schema)
     3. `tsx prisma/seed.ts` (seeds all 8 pharmacies, ~50 medicines, demo delayed order, and demo accounts)
     4. `next build` (generates static and dynamic pages)

---

## ⚡ Option B: Deploy via Netlify CLI

If you have the [Netlify CLI](https://docs.netlify.com/cli/get-started/) installed:

```bash
cd medireach_3

# Sign in to your Netlify account
npx netlify login

# Initialize and link site
npx netlify init

# Build and deploy straight to production
npx netlify deploy --build --prod
```

---

## 📁 Option C: Deploy via Netlify Drop (Drag & Drop)

1. Build the project locally:
   ```bash
   cd medireach_3
   npm install
   npm run build
   ```
2. In the Netlify web dashboard, navigate to **Sites** → **"Want to deploy a new site without connecting to Git? Drag and drop your site output folder here"**.
3. Upload the generated `.next` and project folder or use Netlify CLI.

---

## 🔑 Pre-Configured Test Accounts on Deployed Site

Once your Netlify URL is live (e.g. `https://your-site.netlify.app`), you can switch personas using the **Demo Role Switcher** at the top of every page, or sign in directly:

| Role | Email | Password | Quick Dashboard Link |
| :--- | :--- | :--- | :--- |
| **Customer** | `customer@medireach.in` | `Password123!` | `/` (Home / Search / Cart) |
| **Apollo Owner** | `apollo@medireach.in` | `Password123!` | `/pharmacy/dashboard` |
| **MedPlus Owner** | `medplus@medireach.in` | `Password123!` | `/pharmacy/dashboard` |
| **Delivery Rider** | `delivery@medireach.in` | `Password123!` | `/delivery/dashboard` |
| **Admin** | `admin@medireach.in` | `Password123!` | `/admin/dashboard` |

---

## 🧪 Post-Deployment Verification Checklist

1. **Homepage & Catalog**: Visit `/` to see medicines with live stock indicators across Yelahanka stores.
2. **Delay Fallback Scenario**: Visit `/orders/demo` to test the active delayed order and 1-click pharmacy rerouting on the interactive Leaflet map.
3. **Trigger A Cart Out-of-Stock Fallback**: Add Azithromycin from Apollo to cart; notice the immediate warning banner offering nearby MedPlus with 1-click switch.
4. **Prescription Verification**: In `/pharmacy/dashboard`, review customer prescriptions and approve/reject with notes.
5. **AI Health Assistant**: Ask questions at `/ai-assistant` and observe pharmacology responses with safety disclaimers and emergency triage.
