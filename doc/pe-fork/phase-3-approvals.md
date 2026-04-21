# Phase 3 — Managing Partner approval controls

**Status:** ⚪ not started
**Goal:** Give the Managing Partner (human) a per-agent or per-task switch between **autonomous** execution and **requires approval**, with a clean approval queue that works on mobile.

---

## In scope

- Audit what approval/governance primitives Paperclip already has and reuse them. (Paperclip advertises approval gates + rollback already — likely most of the mechanism exists.)
- Per-agent setting: `execution_mode = autonomous | requires_approval`.
- Per-task override (optional): a specific task can require approval even if the agent is autonomous.
- Approval queue page:
  - List of pending approvals with agent, task, summary, proposed action, cost estimate
  - Approve / reject / ask-for-changes buttons
  - Works on mobile viewport
- Audit trail: every approval/rejection recorded with who, when, and reasoning (optional note).
- Default for all PE agents shipped in Phase 2: `requires_approval` (safer opt-in to autonomy).

## Out of scope

- Multi-human approval workflows (you are the only human).
- Conditional approval rules (e.g., "auto-approve if cost < $5"). Might revisit later.

---

## Tasks

- [ ] Survey existing Paperclip governance / approval code; write a findings note in this folder
- [ ] Add `execution_mode` to agent record (if not present)
- [ ] Add per-task override (if not present)
- [ ] Build / polish approval queue UI; verify mobile viewport
- [ ] Set Phase 2 PE agents' defaults to `requires_approval`
- [ ] Smoke test: start a run as autonomous, observe it complete; switch to requires-approval, confirm it pauses for human input
- [ ] Commit + push
- [ ] Flip Phase 3 status to 🟢

---

## Open questions

- What granularity of "task" triggers an approval? Every tool call would be intolerable. Lean: every **run** / major decision (e.g., "produce diligence report", "publish memo") gets one approval gate.
- Cost estimates: surface before or only after? (Lean: show estimate up-front where possible.)
- Approval via push notification? Email? Initially just in-app; defer push notifications.

## Risks

- Approval fatigue. If every task needs a tap, the Managing Partner stops paying attention. Keep gate count low and bundle related sub-steps under one approval.

## Done criteria

1. Flipping an agent from autonomous to requires-approval stops its next run pending a human tap.
2. Approving a pending task releases it and it completes normally.
3. Approval queue usable on mobile.
4. Phase 3 pushed to GitHub.
