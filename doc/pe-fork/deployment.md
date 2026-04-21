# gcOS on Railway — Deployment Guide

This is the step-by-step for deploying gcOS (this fork of Paperclip) to Railway. It's the first place the Managing Partner can actually use the PE OS over HTTPS.

We pulled this deploy forward from Phase 6 because local dev isn't the chosen path for this operator.

---

## What's in the repo for Railway

- `Dockerfile` at the repo root — Railway uses this to build.
- `railway.toml` at the repo root — points Railway at the Dockerfile, sets the healthcheck path to `/api/health`, and configures restart policy.
- The Dockerfile exposes `3100` and honors `$PORT` at runtime, so Railway's injected port works.

## What you do in Railway's UI

You only need to do this once.

### 1. Create the project
1. Go to [railway.app](https://railway.app) → log in with GitHub.
2. **New Project → Deploy from GitHub repo → `mpansky/gcOS`**.
3. When Railway asks which branch, pick **`claude/brainstorm-repo-modifications-GRXq2`** (the working branch for this fork) or `main` — whichever you want to track. For active development, track the working branch.
4. Railway starts a first build. It'll likely fail or boot unhealthy — that's expected. We have setup left to do.

### 2. Add a Postgres database
1. In the project → **+ New → Database → Add PostgreSQL**.
2. Click the Postgres service → **Variables** tab → copy `DATABASE_URL` (use the one labeled for the app's internal network — starts with `postgresql://...railway.internal`).
3. Go to the **gcOS service → Variables → New Variable**:
   - Name: `DATABASE_URL`
   - Value: `${{Postgres.DATABASE_URL}}` (Railway's variable-reference syntax — it auto-wires).

### 3. Add a persistent volume
Paperclip stores instance data, secrets key, uploaded assets in `PAPERCLIP_HOME` (`/paperclip` in the image). Without a volume, that data is wiped on every redeploy.

1. gcOS service → **Settings → Volumes → + New Volume**.
2. Mount path: `/paperclip`.
3. Size: 1–5 GB is plenty to start.

### 4. Set environment variables

On the gcOS service → **Variables**, add:

| Name | Value | Why |
|------|-------|-----|
| `DATABASE_URL` | `${{Postgres.DATABASE_URL}}` | external managed Postgres instead of embedded |
| `PAPERCLIP_HOME` | `/paperclip` | matches the volume mount |
| `PAPERCLIP_DEPLOYMENT_MODE` | `authenticated` | run with auth (the Dockerfile already defaults to this) |
| `PAPERCLIP_DEPLOYMENT_EXPOSURE` | `public` | Railway's domain is public; override the Dockerfile's `private` default |
| `HOST` | `0.0.0.0` | listen on all interfaces (Dockerfile already sets this, belt-and-suspenders) |
| `PAPERCLIP_API_URL` | `https://<your-railway-domain>` | fill in after step 5 below, then redeploy |

Do **not** set `PORT` — Railway injects it and the server reads it from env.

Don't set `PAPERCLIP_SECRETS_MASTER_KEY` manually for the first boot; the server will generate one at `/paperclip/.../secrets/master.key` (persisted via the volume). If you want to bring your own, see `docs/deploy/secrets.md`.

### 5. Generate the public domain
1. gcOS service → **Settings → Networking → Generate Domain**.
2. Railway hands you a URL like `gcos-production.up.railway.app`.
3. Copy that URL back into `PAPERCLIP_API_URL` (with `https://` prefix) in Variables. Redeploy.

### 6. Deploy
Railway redeploys automatically when:
- Variables change
- A commit lands on the connected branch

If nothing is redeploying, click **Deployments → Deploy** to force it.

## Verifying it works

Once the build is green:

```sh
curl -i https://<your-railway-domain>/api/health
```

Expected: HTTP 200.

Then open `https://<your-railway-domain>` in a browser. In authenticated mode, you'll be prompted to complete first-run setup (user creation, etc.).

## If something is wrong

- **Build fails:** check Deployments → Build logs. Most common: lockfile drift. Push a fresh commit to retrigger.
- **Boots then crashes:** check Deployments → Deploy logs. Likely missing env var or `DATABASE_URL` not reachable from service.
- **502/unhealthy:** healthcheck at `/api/health` is failing. Check deploy logs for migration errors (a schema mismatch against the managed Postgres is the usual culprit — in that case, connect with a psql client and verify the DB is reachable).
- **"Private exposure" auth wall blocking you:** you forgot `PAPERCLIP_DEPLOYMENT_EXPOSURE=public`.

## Costs (ballpark)

- gcOS service: ~$5/mo at starter usage.
- Postgres: ~$5/mo for the smallest managed instance.
- Volume: negligible at 1–5 GB.
- Railway trial credits usually cover the first month.

Keep an eye on **Usage** in Railway; autonomous agents can rack up CPU/egress fast if a heartbeat loops.

## When to revisit

Phase 6 will formalize this deployment (add CI via GitHub Actions, custom domain if desired, backup strategy). For now, this gets you a live URL so you can use gcOS while we build out Phases 1–5.
