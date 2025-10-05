# Deploy instructions for Render (SwordShop)

This project is a Node + TypeScript + Vite + Express app using Drizzle + Neon for Postgres.
Below are the exact settings to create a working Render deployment and connect to a Neon (or other Postgres) database.

## 1) Prepare a Neon (recommended) or other Postgres DB
- Sign up at https://neon.tech and create a new branch/database (Free tier available).
- From Neon, copy the `Connection String` (it will look like: `postgresql://...`).
- Make sure the connection string includes `?sslmode=require` if needed.

## 2) Create a new Web Service on Render
- In Render dashboard, click "New" -> "Web Service".
- Connect your GitHub repo or choose "Manual deploy" and upload the zip created.
- Use the following fields:
  - **Environment**: Docker (optional) or Node (select "Node" for a standard build)
  - **Build Command**: `npm ci && npm run build`
  - **Start Command**: `npm start`
  - **Branch**: main (if using git)
  - **Root Directory**: leave blank if package.json is at repo root

## 3) Environment Variables (in Render > Environment > Add)
- `DATABASE_URL` = (the Neon Postgres connection string)
- `SESSION_SECRET` = a long random string (change it)
- `NODE_ENV` = production
- (Render provides `PORT` automatically; no need to set it)

## 4) Notes about the project
- The server entry is `server/index.ts`. Build command produces `dist/index.js` used by `npm start`.
- The project uses `@neondatabase/serverless` + `drizzle-orm/neon-serverless` — Neon is the easiest DB to use here. If you use Render Postgres, you may need to adjust DB adapter code.
- Static client is built by `vite build` and served by the Express server from `dist` folder.

## 5) Uploading files / local testing
- Locally, run:
  - `npm ci`
  - `npm run build`
  - `npm start`
- To test locally with a Postgres DB, set `DATABASE_URL` in your environment (or use .env with dotenv in development).

## 6) After deploy
- Visit the Render URL and test sign-up/login and product flows.
- If you want a custom domain, add it in Render (Settings -> Domains) and follow DNS instructions at your registrar.

If you want, I can also produce a Render `render.yaml` or adjust the project to use `drizzle-orm` with a different adapter — tell me and I will prepare it.
