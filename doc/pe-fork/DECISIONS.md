# Decisions log

Append-only. Newest at top. Every non-obvious choice in the PE fork lands here with a date and rationale.

Format:
```
## YYYY-MM-DD — Short title
**Decision:** what we chose.
**Why:** 1–3 sentences.
**Alternatives considered:** brief.
```

---

## 2026-04-21 — Phase order: entity model before diligence skills
**Decision:** Build the PE Company entity model (Phase 1) before the diligence skills (Phase 4).
**Why:** Diligence output needs to attach to a Company at a specific stage, with data-room references. Shipping diligence first would require throwaway scaffolding that we'd replace in Phase 1.
**Alternatives considered:** Flip the order to demo diligence quickly. Rejected because it duplicates work.

## 2026-04-21 — Push to GitHub at the end of every phase
**Decision:** Commit and `git push` to the working branch after each phase completes.
**Why:** Past attempts lost work. Every phase is independently useful, so frequent pushes = durable progress.
**Alternatives considered:** One push at the end. Rejected — too risky.

## 2026-04-21 — Deployment host deferred to Phase 6
**Decision:** Run locally for Phases 0–5, pick the host at Phase 6.
**Why:** Paperclip is a stateful Node.js + Postgres app with long-running heartbeats. Netlify alone can't host it. We'll evaluate Railway / Render / Fly.io / Vercel at Phase 6 with a working system in hand.
**Alternatives considered:** Lock in a target now. Rejected — we'd optimize for constraints we haven't felt yet.

## 2026-04-21 — All agents on OpenClaw
**Decision:** The PE firm's agent roster uses OpenClaw as the sole runtime for now.
**Why:** User preference; simplifies adapter config, env, auth, and debugging while we shape the PE workflow. Paperclip supports multiple adapters, so we can add others later without rework.
**Alternatives considered:** Mix Claude Code / Codex / Cursor from day one. Deferred.

## 2026-04-21 — PE fork planning lives in `doc/pe-fork/`, not `docs/`
**Decision:** Put the PE fork tracker under `doc/pe-fork/` (the internal docs tree), not `docs/` (the public docs tree served by docs.json).
**Why:** These plans are internal to the fork, not user-facing Paperclip documentation. Keeping them in `doc/` matches the existing convention (`doc/plans/`, `doc/spec/`).
**Alternatives considered:** `docs/pe-fork/`. Rejected — would pollute public docs.

## 2026-04-21 — Fork stays additive where possible
**Decision:** Prefer new tables / new columns / new modules over editing Paperclip internals.
**Why:** Keeps the door open to merge upstream Paperclip improvements. Easier to reason about blast radius.
**Alternatives considered:** Rewrite Company model in place. Rejected for now — revisit if additive gets ugly.
