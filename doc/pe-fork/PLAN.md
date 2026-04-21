# gcOS — PE Fork Plan

**Fork of:** Paperclip (paperclipai/paperclip)
**Purpose:** Turn Paperclip into a solo-operator Private Equity firm OS, where each Paperclip "Company" represents a portfolio company (prospect, under diligence, or invested). All agents run on OpenClaw.
**Branch:** `claude/brainstorm-repo-modifications-GRXq2`
**Owner:** Managing Partner (human)

---

## Working principles

1. **Incremental.** Previous attempts tried to do everything at once and failed. Each phase ships something that works on its own.
2. **Push after every phase.** Work is committed and pushed to GitHub at the end of each phase so progress is never lost.
3. **Fork-friendly.** Prefer additive changes (new fields, new tables, new modules) over rewriting Paperclip internals, so we can merge upstream later.
4. **Document decisions.** Every non-obvious choice lands in `DECISIONS.md` with a date and rationale.
5. **Markdown first.** Per-phase docs in this folder capture scope, tasks, and open questions before code lands.

---

## Phase list

| # | Phase | Status | Doc |
|---|-------|--------|-----|
| 0 | Fork hygiene & Railway baseline | 🟡 in progress | [phase-0-baseline.md](phase-0-baseline.md) · [deployment.md](deployment.md) |
| 1 | PE Company entity model | ⚪ not started | [phase-1-entity-model.md](phase-1-entity-model.md) |
| 2 | PE org chart template (OpenClaw) | ⚪ not started | [phase-2-org-chart.md](phase-2-org-chart.md) |
| 3 | Managing Partner approval controls | ⚪ not started | [phase-3-approvals.md](phase-3-approvals.md) |
| 4 | Diligence skills (data room + board deck) | ⚪ not started | [phase-4-diligence.md](phase-4-diligence.md) |
| 5 | Per-company memory / knowledge | ⚪ not started | [phase-5-memory.md](phase-5-memory.md) |
| 6 | Deployment hardening (CI, domain, backups) | ⚪ not started | [phase-6-deploy.md](phase-6-deploy.md) |

> **Deploy target:** Railway. The initial deploy is folded into Phase 0 so the Managing Partner has a live URL from day one. Phase 6 now focuses on hardening (GitHub Actions CI, custom domain, backup strategy) rather than picking a host.

Legend: ⚪ not started · 🟡 in progress · 🟢 done · 🔴 blocked

---

## What this system is (one-pager)

A single-operator Private Equity OS. The human is the **Managing Partner**. OpenClaw agents fill all other seats:

- **Firm-wide roles:** Deal Sourcer, Screening Analyst, Diligence Lead (+ Financial / Market / Legal sub-analysts), IC Devil's Advocate, Memo Writer
- **Per-portco roles:** Board Advisor, Operating Partner, Portfolio Monitor

Each portfolio company lives in Paperclip as a `Company` record with a PE stage (`sourced → screening → diligence → ic → invested → monitoring → exit`), a strategy type (growth equity / rollup / VC), and an attached data room. Agents work on each company according to its current stage.

The Managing Partner decides per-agent (or per-task) whether work runs autonomously or requires approval. Approvals are mobile-friendly.

The headline capability is **diligence**: point an agent at a data-room folder and get a scored report; drop in a monthly board deck and get consistency checks + questions for the board.

---

## Out of scope (for now)

- Fundraising / LP onboarding workflow
- Fund accounting / NAV
- Cap table management (use Carta etc.)
- Deal room for buyers/sellers other than us
- Multi-partner workflows (we model solo operator first)

These are not ruled out forever — just not in the first six phases.

---

## How to use this tracker

- **Starting a session?** Read this file first, then the `phase-N-*.md` for the current phase.
- **Making a decision?** Append to `DECISIONS.md` with date and a 1–3 sentence rationale.
- **Finishing a phase?** Flip its status to 🟢 here, commit, push.
- **Blocked?** Flip to 🔴 and write the blocker in the phase doc.
