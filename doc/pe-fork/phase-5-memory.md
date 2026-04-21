# Phase 5 — Per-company memory / knowledge

**Status:** ⚪ not started
**Goal:** Give each Company its own accumulating knowledge store so the Board Advisor, Operating Partner, and Portfolio Monitor get smarter over time instead of starting from scratch every heartbeat.

---

## In scope

- Per-Company memory store containing:
  - Prior diligence reports (from Phase 4)
  - Prior board deck reviews
  - Past Q&A threads with the Managing Partner
  - Key facts extracted from past documents (cap table, board members, strategy pillars, promised milestones)
- Memory write path: each skill run appends relevant facts + a pointer to the full artifact.
- Memory read path: skills pull relevant context at the start of each run.
- Bounded size: cap how much context is loaded; prefer structured facts over raw text when possible.

## Out of scope

- Cross-company / firm-wide memory (e.g., sector trends). Revisit after single-company memory proves useful.
- A full vector DB / RAG stack. Start with Postgres + simple keyword / recency retrieval; upgrade only if retrieval quality bites.
- Knowledge graph. Deferred.

---

## Tasks

- [ ] Read `doc/memory-landscape.md` and `doc/plans/2026-03-17-memory-service-surface-api.md` — see what Paperclip already offers
- [ ] Decide: reuse Paperclip's memory surface vs. add a per-Company table scoped to the PE fork (capture in DECISIONS.md)
- [ ] Schema / API for read + write
- [ ] Hook Phase 4 diligence skills to write a structured summary + attach full artifact
- [ ] Hook Phase 4 skills to read prior context before generating
- [ ] Test: run board review two months in a row; confirm month 2 references month 1 findings
- [ ] Commit + push
- [ ] Flip Phase 5 status to 🟢

---

## Open questions

- Fact extraction: let the LLM do it, or use regex/structured extraction? (Lean: LLM with a strict schema, so we can ship fast and tighten later.)
- Eviction: do we ever drop memory, or keep forever? (Lean: keep forever for now; cost of Postgres rows is negligible at this scale.)
- Who can read a company's memory? Only agents linked to that company, never cross-leak. Enforce at query time.

## Risks

- Memory poisoning: a bad extraction persists and misleads future runs. Mitigation: store provenance (which run produced each fact) so we can audit and purge.
- Leaking portco data across companies. Mitigation: every memory query filters by `company_id` at the DB layer, not just in application code.

## Done criteria

1. Running a board review produces memory entries scoped to the Company.
2. Re-running the same skill a month later demonstrably uses prior entries.
3. Memory is never visible across companies.
4. Phase 5 pushed to GitHub.
