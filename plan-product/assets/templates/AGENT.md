# Agent instructions

Read this before every implementation task. Keep it short — it is loaded every
time.

## Authority order

When documents disagree, higher wins:

`planning/04-rules.md` and `planning/05-constraints.md` are both mandatory. Neither can be
overridden by anything below, and neither outranks the other — they describe
different dimensions. If they contradict each other, that is a planning
conflict: stop and resolve it in the files.

Everything else is ordered:

```
planning/01-product.md
planning/04-rules.md
planning/05-constraints.md
        mandatory dimensions — none can be overridden by anything below,
        and none outranks the others. A conflict among them is a planning
        conflict: stop and resolve it in the files.

      > planning/02-model.md
      > planning/03-screens.md
      > planning/06-journeys.md
      > planning/slices/<slice>.md — spec
      > planning/slices/<slice>.md — tasks
```

`decisions.md` is history, not authority. It records what was decided and why;
the current spec files must already reflect it. If a decision entry disagrees
with a spec file, the spec file was never updated — fix it.

Contradictions are expected, not exceptional. Never resolve one silently.

- If the higher-authority document determines the answer: follow it, **then
  update the lower-authority document to match**, record the change in
  `decisions.md`, and continue. Following the higher document without fixing
  the lower one leaves the contradiction in place for the next task to hit —
  and the next agent may resolve it the other way.
- If nothing determines the answer, it is a gap, not a judgment call. Record it
  in `decisions.md` and stop the affected task.

"Choose whatever seems most consistent" is how invented behavior enters a
product. The test is whether a document decides it, not whether a decision
seems low-risk.

## Per-task loop

1. Read `01-product.md`, `04-rules.md`, `05-constraints.md`, the slice, and
   every model, screen, and journey ID the task references.
2. Identify which constraints in `05-constraints.md` apply to this task —
   performance budget, minimum width, accessibility level, data handling. Note
   them before writing code; they are hard to retrofit.
3. Read the existing code before proposing changes.
4. Implement the smallest complete vertical change — user-visible behavior,
   not backend only.
5. Write tests. Every covered action gets appropriate automated coverage —
   unit, integration, or end-to-end, whichever actually exercises the
   behavior. Every journey gets end-to-end coverage. Do not manufacture a unit
   test to satisfy the wording.
6. Verify each acceptance criterion in the slice file explicitly, one by one.
7. Verify each constraint identified in step 2. A constraint nobody tested is
   a constraint nobody met.
8. Re-run the consistency pass in `planning/07-completeness.md` against the
   files as they now stand.
9. Log any planning gap found to `decisions.md`.

## Never

- Add an entity, field, screen, or action that is not in `planning/02-model.md`
  or `planning/03-screens.md`. Propose it instead.
- Change the meaning of any planning file without recording a `D-nnn` entry in
  `decisions.md`. You are working after handoff, so every semantic change is
  one the plan's author has not seen. Typos and formatting need no entry.
- Mark a task done with an acceptance criterion or applicable constraint
  unverified.
- Satisfy a reference by mentioning an ID. A `refs:` entry counts only when the
  work actually implements or verifies that behavior.

## Slice close

A slice is not done when the tasks pass. It is done when a human has used it.

The review checklist at the bottom of each slice file is filled in by the user,
not by the agent — discoverability and "does this match what I imagined" are
not things an agent can assess about its own output. Ask for the review; do not
self-certify it.
