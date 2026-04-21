# Phase 6 — Deployment

**Status:** ⚪ not started
**Goal:** Move gcOS off the Managing Partner's laptop and onto a host where it can run 24/7, with CI wired to the GitHub repo.

---

## Architectural reality check

Paperclip is **not** a static site. It is:
- A long-running Node.js server
- An embedded (local) or external Postgres database
- Scheduled heartbeats (needs a persistent process, or an external scheduler)
- Persistent agent sessions and file storage

**Netlify by itself cannot host this.** Realistic options:

| Option | Server | DB | Scheduler | Notes |
|--------|--------|-----|-----------|-------|
| A. Railway / Render / Fly.io | long-running Node process | managed Postgres | in-process | closest to current architecture; least surgery |
| B. VPS (Hetzner, DigitalOcean) | Docker container | managed or co-hosted Postgres | in-process | full control, more ops |
| C. Vercel + Neon/Supabase | serverless functions | Neon/Supabase | external cron (Upstash / Vercel cron) | requires splitting the server; more work |
| D. Netlify (static) + anything above | only the landing page on Netlify | — | — | Netlify for marketing page only |

Decision deferred to this phase on purpose — we'll pick once we feel the actual runtime cost and latency of the system.

## In scope

- Pick a host (lean: A, specifically Railway for simplicity).
- Set up managed Postgres.
- Environment config (secrets, OpenClaw keys, telemetry opt-out if desired).
- GitHub Actions CI: typecheck + test on PR.
- Deploy-on-push to the working branch (for the solo user's convenience).
- Smoke test: onboarded, running PE firm available at a URL over HTTPS.

## Out of scope

- Multi-region / HA. Solo operator; one region is fine.
- Team access controls / SSO. Solo operator.
- Backups beyond what the managed Postgres provides by default.

---

## Tasks

- [ ] Make the decision; capture in DECISIONS.md
- [ ] Provision host + managed Postgres
- [ ] Move DB from embedded → managed; run migrations
- [ ] Configure env vars (OpenClaw, Paperclip, any storage keys)
- [ ] Wire GitHub Actions (typecheck, test, optional deploy)
- [ ] DNS / custom domain (optional)
- [ ] Smoke test: create a company, run a heartbeat, land a diligence report
- [ ] Document the running deployment in `doc/pe-fork/deployment.md` (so future-you can rebuild it)
- [ ] Commit + push
- [ ] Flip Phase 6 status to 🟢

---

## Open questions

- Do we want the server's data-room references to be local paths (host-machine disk) or cloud (S3 / R2 / Drive)? If cloud, more setup here.
- Secret management: host's env vars vs. a dedicated secrets manager. (Lean: host env vars for solo use.)

## Risks

- Heartbeat cadence that worked locally may surprise us under network latency. Watch and tune.
- Managed Postgres cost ramp if the DB grows. Monitor.

## Done criteria

1. gcOS reachable over HTTPS at a stable URL.
2. A heartbeat on a cloud OpenClaw agent runs successfully end-to-end.
3. CI runs on PRs to the working branch.
4. Phase 6 pushed to GitHub, `PLAN.md` fully green.
