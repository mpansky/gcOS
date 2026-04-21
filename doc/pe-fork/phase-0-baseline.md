# Phase 0 — Fork hygiene & baseline

**Status:** 🟡 in progress
**Goal:** Start from a known-good Paperclip baseline with the fork tracker in place, and confirm the local dev loop works end-to-end before we change anything.

---

## In scope

- Create the `doc/pe-fork/` tracker (this file lives here).
- Verify local dev works: `pnpm install`, `pnpm dev`, UI loads at `http://localhost:3100`, embedded Postgres boots, migrations run clean.
- Hire one OpenClaw agent end-to-end (heartbeat lands, task gets picked up, logs appear). This is a smoke test, not a PE feature.
- Commit + push the scaffold.

## Out of scope

- Any PE-specific schema or UI changes (that's Phase 1+).
- Rebranding Paperclip → gcOS in the UI (revisit in Phase 6 or when noise warrants).
- Picking a deployment host (Phase 6).

---

## Tasks

- [x] Create `doc/pe-fork/PLAN.md`
- [x] Create `doc/pe-fork/DECISIONS.md`
- [x] Create per-phase docs (phases 0–6)
- [ ] **User:** run `pnpm install && pnpm dev` locally; confirm UI loads and migrations pass
- [ ] **User:** onboard one OpenClaw agent via the existing flow; confirm at least one heartbeat reaches the server
- [ ] Commit and push Phase 0 scaffold to `claude/brainstorm-repo-modifications-GRXq2`
- [ ] Flip Phase 0 status to 🟢 in `PLAN.md` once baseline is verified

---

## Open questions

- Do we want the UI to show a "PE mode" label anywhere in Phase 0, or keep the UI untouched until Phase 1 adds real PE fields? *(Default: untouched.)*
- Is there a specific OpenClaw config (model, budget, skill set) you want every PE agent to start from, or do we configure per-role in Phase 2?

## Notes / gotchas

- Paperclip needs Node 20+ and pnpm 9.15+.
- Don't be surprised if the first `pnpm dev` spends a minute initializing the embedded Postgres.
- If onboarding fails, `doc/OPENCLAW_ONBOARDING.md` is the reference.

## Done criteria

1. `pnpm dev` boots cleanly on the user's machine.
2. One OpenClaw agent has posted at least one heartbeat.
3. This file's task checklist is fully checked off.
4. Phase 0 scaffold is pushed to GitHub.
