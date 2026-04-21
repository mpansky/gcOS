# Phase 0 — Fork hygiene & baseline (Railway path)

**Status:** 🟡 in progress
**Goal:** Get gcOS running at a live Railway URL with its own managed Postgres, so the Managing Partner has a working environment. This replaces the "verify local dev" baseline we originally planned.

See `deployment.md` in this folder for the step-by-step Railway setup.

---

## In scope

- Create the `doc/pe-fork/` tracker.
- Add Railway config (`railway.toml`) so Railway picks up the existing `Dockerfile`.
- Deploy to Railway with managed Postgres + persistent volume at `/paperclip`.
- Hit `/api/health` on the Railway URL and get HTTP 200.
- Onboard one OpenClaw agent end-to-end (one heartbeat lands).

## Out of scope

- Any PE-specific schema or UI changes (Phase 1+).
- Rebranding Paperclip → gcOS in the UI.
- Custom domain / CI / backups (Phase 6 formalizes these).

---

## Tasks

- [x] Create `doc/pe-fork/PLAN.md`
- [x] Create `doc/pe-fork/DECISIONS.md`
- [x] Create per-phase docs (phases 0–6)
- [x] Add `railway.toml` and `doc/pe-fork/deployment.md`
- [x] Push Phase 0 scaffold to `claude/brainstorm-repo-modifications-GRXq2`
- [ ] **User:** connect Railway to the GitHub repo (`mpansky/gcOS`, branch `claude/brainstorm-repo-modifications-GRXq2`)
- [ ] **User:** add the Postgres plugin; wire `DATABASE_URL`
- [ ] **User:** add a 1–5 GB volume mounted at `/paperclip`
- [ ] **User:** set env vars per `deployment.md`
- [ ] **User:** generate a Railway public domain
- [ ] **User:** `curl https://<railway-domain>/api/health` returns 200
- [ ] **User:** open the UI, complete first-run setup
- [ ] **User:** hire one OpenClaw agent; confirm a heartbeat lands (check logs)
- [ ] Flip Phase 0 status to 🟢 in `PLAN.md`

---

## Open questions

- Do we want the UI to show a "PE mode" label anywhere in Phase 0, or keep the UI untouched until Phase 1 adds real PE fields? *(Default: untouched.)*
- Is there a specific OpenClaw config (model, budget, skill set) you want every PE agent to start from, or do we configure per-role in Phase 2?
- OpenClaw adapter from the Railway container: the Dockerfile pre-installs `claude` and `codex` CLIs but not OpenClaw. We'll likely wire OpenClaw as a remote adapter (HTTP-style, talking back to Railway). Confirm when we get there.

## Notes / gotchas

- The Dockerfile defaults to `PAPERCLIP_DEPLOYMENT_EXPOSURE=private`. Override to `public` on Railway or the UI will refuse requests over the Railway domain.
- Railway injects `PORT`; don't hardcode it in env vars.
- First boot on managed Postgres runs migrations automatically, but if the DB isn't reachable the container will crash — watch deploy logs.

## Done criteria

1. `https://<railway-domain>/api/health` returns 200 consistently.
2. One OpenClaw agent has posted at least one heartbeat against the Railway URL.
3. Phase 0 scaffold + Railway config pushed to GitHub.
4. Managing Partner has logged into the UI.
