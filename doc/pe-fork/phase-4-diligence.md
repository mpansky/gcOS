# Phase 4 — Diligence skills

**Status:** ⚪ not started
**Goal:** Ship the two diligence capabilities the Managing Partner cares about most: (1) evaluate a prospective company's data-room folder during screening/diligence, and (2) review a monthly board deck for an invested company and challenge it with pointed questions.

This is the headline value of the PE fork.

---

## In scope

### Skill A — Data-room diligence
- Input: a folder (local path, storage ref, or cloud-drive link) of documents belonging to a prospective or in-diligence Company.
- Output: a structured diligence report attached to the Company, scored across:
  - Market (size, growth, competition)
  - Financials (revenue, margins, growth, burn)
  - Legal / risk (red flags in contracts, IP, compliance)
  - Team (key people, gaps)
  - Product / tech (for growth-equity or VC plays)
  - Overall recommendation with confidence
- Wired to roles: Diligence Lead orchestrates, sub-analysts do their specialties, Memo Writer turns output into an IC-ready memo.

### Skill B — Board deck review
- Input: a current monthly board deck (PDF/slides) + prior N months of decks + known company financials.
- Output: attached to the Company with:
  - KPI consistency checks (numbers agree across slides, agree with prior decks, agree with financials)
  - Forward-looking strategy check (is the plan consistent with past commitments? are projections realistic given history?)
  - A prioritized list of **questions for the board** the Managing Partner should ask
- Wired to: Portfolio Monitor (consistency), Board Advisor (strategic questions), Operating Partner (execution reality check).

### Shared mechanics
- Document ingestion pipeline (PDF → text, XLSX → table, DOCX → text).
- Per-Company attachment of reports (so they're visible on the Company detail page).
- Report format defined once as a template, reused.

## Out of scope

- Building full financial models (LBO/DCF) from scratch — defer.
- Live data pulls from QuickBooks / Stripe — defer (Phase 5 or later).
- OCR for scanned/image PDFs — start with text-native PDFs, revisit if needed.

---

## Tasks

- [ ] Design the diligence report schema (one markdown in this folder)
- [ ] Design the board-deck-review schema
- [ ] Build document ingestion helper (reuse existing Paperclip if present; otherwise a thin utility)
- [ ] Implement Skill A (data-room diligence) as an OpenClaw skill
- [ ] Implement Skill B (board deck review) as an OpenClaw skill
- [ ] Wire skills to the relevant roles defined in Phase 2
- [ ] UI: show latest diligence report on Company detail page
- [ ] UI: show latest board review on Company detail page
- [ ] End-to-end test with a real (or realistic-synthetic) data room
- [ ] End-to-end test with three months of synthetic board decks
- [ ] Commit + push
- [ ] Flip Phase 4 status to 🟢

---

## Open questions

- Where do documents live? Local filesystem of the Paperclip host? Cloud drive link? Upload to Paperclip's storage? (Default: local filesystem path referenced on Company, upload UI later.)
- Do we need to redact sensitive data before it hits an LLM? (Start: assume the Managing Partner controls what goes in and accepts the risk. Revisit for production.)
- Cost containment: a full data-room pass could be large. Enforce per-run budget and a document-count cap.

## Risks

- Hallucination on financials. Structured extraction + "show your work" / cite-the-source-doc should be required of the skill.
- PDF parsing quality varies. Have a fallback "I couldn't parse this, please re-upload" path.

## Done criteria

1. Point the Diligence Lead at a sample data room → receive a scored report attached to the Company.
2. Point the Portfolio Monitor at three months of decks → receive a consistency report + questions-for-the-board doc.
3. Reports visible on the Company detail UI.
4. Phase 4 pushed to GitHub.
