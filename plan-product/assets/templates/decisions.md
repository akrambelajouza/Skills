# Decisions

Append-only. Newest at the bottom. Never edit or delete an entry — a
superseded decision gets a new entry that references the old one.

Two kinds of entry go here:

- **Gaps** — something the plan did not determine, found during planning or
  implementation.
- **Decisions** — a choice made, with what was rejected and why.

When a decision entry belongs here depends on where you are:

- **During planning**, before the package is handed to a coding agent: fix
  wrong or unclear specs directly. No entry.
- **After handoff**, once implementation has started: a change to the meaning
  of `01-product.md`, `02-model.md`, `03-screens.md`, `04-rules.md`, or
  `05-constraints.md` gets both — update the file *and* record a `D-nnn` entry.
  The coding agent has already read the old version.
- **Either way**, typos, formatting, and clarifications that do not change
  meaning need no entry.

---

## D-001 — Export deferred

**Date:** YYYY-MM-DD
**Context:** Entity action checklist flagged export as unanswered for E-Task.
**Decision:** Not in v1.
**Reason:** No user need identified yet; adding a format now would constrain
the data model.
**Revisit:** If a user asks for it, or before any integration work.

---

## D-004 — Notifications deferred to v2

**Date:** YYYY-MM-DD
**Context:** Product capability checklist, phase 1.
**Decision:** No notifications in v1.
**Reason:** Requires a delivery channel and a scheduling story; neither is
worth building before the core loop is proven.
**Revisit:** Once users report missing due dates.

---

## D-005 — Data export deferred

**Date:** YYYY-MM-DD
**Context:** Product capability checklist, phase 1.
**Decision:** No export in v1.
**Reason:** Committing to a format now would constrain the data model before it
has settled.
**Revisit:** Before any integration work, or on first user request.

---

## D-006 — <next>
