# Phase 2 — PE org chart template (OpenClaw)

**Status:** ⚪ not started
**Goal:** Ship a reusable "PE firm" company template (in Paperclip's import/export format) that bootstraps the full agent roster for the firm, all configured to run on OpenClaw.

---

## In scope

- Define the firm-wide org chart as Paperclip agents:
  - Managing Partner (the human user — seat at the top)
  - Deal Sourcer
  - Screening Analyst
  - Diligence Lead
    - Financial Analyst
    - Market Analyst
    - Legal Analyst
  - IC Devil's Advocate
  - Memo Writer
- Define **per-portco** roles as a template that gets instantiated when a Company enters `invested`:
  - Board Advisor
  - Operating Partner
  - Portfolio Monitor
- Each role ships with:
  - A job description (`AGENTS.md`-style)
  - A default OpenClaw adapter config
  - A starter skill set (can be empty; filled in Phase 4 for diligence roles)
  - A budget default (sensible conservative starting cap)
  - A heartbeat schedule default
- Package everything as an importable company template (reuse Paperclip's export/import).

## Out of scope

- The skills themselves (Phase 4 delivers the diligence skills; other skills come later).
- Approval workflow (Phase 3).
- Actual memory wiring (Phase 5).

---

## Tasks

- [ ] Read `doc/plans/2026-03-13-company-import-export-v2.md` and related to understand template format
- [ ] Draft role definitions (one markdown file per role, co-located in this folder or under `skills/`)
- [ ] Build the PE firm template file
- [ ] Add "instantiate per-portco roles" hook that fires when a Company transitions to `invested`
- [ ] Test: import the template into a fresh Paperclip instance; confirm all firm-wide agents appear
- [ ] Test: move a company to `invested`; confirm per-portco agents get created and linked to the company
- [ ] Commit + push
- [ ] Flip Phase 2 status to 🟢

---

## Open questions

- Should per-portco agents be separate agent records per company, or shared agents that context-switch by company? (Lean: separate, so memory and budgets don't bleed across portcos.)
- Heartbeat frequency defaults: daily for Diligence during active diligence, weekly for Portfolio Monitor, monthly for Board Advisor (around board meetings). Confirm?
- Budget defaults: should firm-wide agents share a pooled budget, or each have their own? (Lean: each their own, small. Easier to isolate runaway loops.)

## Risks

- Per-portco role instantiation on stage change is a workflow hook — needs to be idempotent (don't duplicate agents if a company flips back and forth).
- Agent count can explode once you have 20 portcos. Keep per-portco role defaults light.

## Done criteria

1. A single import creates the full firm roster.
2. Transitioning a company to `invested` creates Board Advisor + Operating Partner + Portfolio Monitor linked to it.
3. All agents successfully send at least one heartbeat on OpenClaw.
4. Phase 2 pushed to GitHub.
