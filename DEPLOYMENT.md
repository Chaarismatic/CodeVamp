# Deployment Guide (Railway + Vercel)

This guide deploys:
- `apps/api` (NestJS backend) on **Railway**
- `apps/web` (Vite React frontend) on **Vercel**

---

## 1) Prerequisites

1. Push this repo to GitHub.
2. Create a MongoDB Atlas cluster.
3. In Atlas, allow network access (`0.0.0.0/0`) or add Railway egress IPs.
4. Prepare environment values:
   - `MONGODB_URI`
   - `JWT_SECRET`

---

## 2) Deploy Backend on Railway

You already have a Railway config file at root: `railway.json`.

1. In Railway dashboard, choose **New Project** -> **Deploy from GitHub Repo**.
2. Select your repo.
3. Railway reads `railway.json` for build/start commands.
4. Add env vars in service settings:
   - `MONGODB_URI`
   - `JWT_SECRET`
  - `NODE_ENV=production`
5. Deploy.

After deploy, copy backend URL:
- Example: `https://codevamp-api-production.up.railway.app`

---

## 3) Deploy Frontend on Vercel

Use the root `vercel.json` included in this repo.

1. In Vercel, import the same GitHub repo.
2. During setup, keep **Root Directory = /** (repo root).
3. Add env var:
   - `VITE_API_URL=https://<your-railway-backend-domain>`
   - Example: `VITE_API_URL=https://codevamp-api-production.up.railway.app`
4. Deploy.

---

## 4) CORS + API wiring

- Backend currently uses `app.enableCors()` (open CORS), so Vercel -> Railway requests work out of the box.
- Frontend reads API base URL from:
  - `apps/web/src/config.ts`
  - `VITE_API_URL` in production should always be set for Vercel deployment.

---

## 5) Verify after deployment

1. Open frontend URL (Vercel).
2. Register/login.
3. Open any problem and run:
   - **Run Tests**
   - **Submit**
4. Confirm APIs return data:
   - `/problems`
   - `/submissions/execute`
   - `/submissions/status/:jobId`

---

## Notes

- Netlify-specific files were removed from this setup.
- WebSocket leaderboard can be less reliable if Railway service sleeps on low-usage plans; REST APIs continue to work.
