# Phase 1 — PE Company entity model

**Status:** ⚪ not started
**Goal:** Extend Paperclip's `Company` entity so each record can represent a portfolio company in the PE lifecycle, with enough structured data for agents to act on it.

---

## In scope

- Add PE-specific fields to `Company` (or a sibling table — to be decided during design):
  - `stage` enum: `sourced | screening | diligence | ic | invested | monitoring | exit`
  - `strategy_type` enum: `growth_equity | rollup | vc`
  - `sector` (free text or enum — TBD)
  - `check_size` (numeric, nullable)
  - `ownership_pct` (numeric, nullable — only meaningful once invested)
  - `investment_date` (date, nullable)
  - `lead_partner` (human user ref — defaults to Managing Partner)
  - `data_room_ref` (string — path, storage key, or external URL)
- DB migration + Zod/Prisma (whatever Paperclip uses) schema updates.
- API endpoints: read + write PE fields.
- UI: Company detail page shows PE fields; list view groups by `stage` (kanban-style).
- Seed one example portfolio company so Phase 2+ has something to work against.

## Out of scope

- Agent roles (Phase 2).
- Approval workflow (Phase 3).
- Diligence skills (Phase 4).
- Per-company memory (Phase 5).

---

## Tasks

- [ ] Read Paperclip's existing Company schema and API surface; write a short `design.md` note in this folder summarizing what we're extending and how
- [ ] Decide: extend `companies` table vs. add a `portfolio_companies` sibling table keyed by `company_id` (capture in DECISIONS.md)
- [ ] Write DB migration
- [ ] Update server types / API handlers
- [ ] Update UI: Company detail page
- [ ] Update UI: Company list / kanban by stage
- [ ] Seed example portco
- [ ] Manual smoke test: create, edit, move across stages
- [ ] Commit + push
- [ ] Flip Phase 1 status to 🟢

---

## Open questions

- Is `sector` free text or an enum? (Lean: free text + suggestions.)
- Do we model funds at all in Phase 1, or assume a single implicit fund? (Lean: single implicit fund for now.)
- Do we need `currency` per company, or USD everywhere? (Lean: USD-only for v1.)
- Should stage transitions be free, or gated (e.g., can't jump from `sourced` to `invested` without passing through `ic`)? (Lean: free, with a soft warning in UI.)

## Risks

- Paperclip's Company entity is core — additive changes only. If we need to touch core code, log it in DECISIONS.md and keep the diff minimal.
- UI kanban could balloon. Keep it a simple column-per-stage view; polish later.

## Done criteria

1. A Company can be created with full PE fields and persisted.
2. UI shows stage + strategy + sector + check size + ownership + data-room ref.
3. Example portco visible in a stage-grouped list.
4. Migration runs clean on a fresh DB.
5. Phase 1 pushed to GitHub.
