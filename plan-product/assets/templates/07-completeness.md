# Completeness checks

This file ships with the planning package so it is self-contained. The coding
agent runs the consistency pass from here — it has no access to the planning
skill that produced these documents.

---

## Consistency pass

Run at the close of every task and every slice, not only at the end of
planning. Implementation surfaces new fields and screens, and the moment one
lands in `02-model.md` without a home in `03-screens.md`, the plan has quietly
regressed.

1. **Every action has a home.** Each entity action marked `yes` in
   `02-model.md` appears on at least one screen in `03-screens.md`.
2. **Every field obligation is met.** From `02-model.md`:
   - every field with `On create: user` appears in some screen's Create fields
   - every field with `Editable: yes` appears in some screen's Edit fields
   - every field with `In list: yes` appears in the Display fields of a list
     screen for that entity

   The middle one is the check that would have caught the original missing edit
   button. The other two catch the identical failure at creation and at
   display.
3. **Screens claim nothing the model withholds.** This is check 1 in reverse,
   and it must test the verdict, not merely the existence of a row:
   - every action a screen hosts exists in that entity's table **and has
     verdict `yes`**
   - every field in a screen's Create fields has `On create: user`
   - every field in a screen's Edit fields has `Editable: yes`
   - every field in a list screen's Display fields has `In list: yes`

   A screen hosting `E-Task.delete` while the model says `delete: no` passes a
   mere existence test and is exactly the contradiction worth catching.
4. **The model agrees with itself.** A field obligation implies its action:
   `On create: user` requires `create: yes`; `Editable: yes` requires
   `edit: yes`; `In list: yes` requires `view_list: yes`.
5. **Relationships resolve.** Every relationship names an entity that exists in
   `02-model.md`, with cardinality and an `On target delete` behavior stated.
6. **Every screen has a verdict on all five states** — loading, ready, empty,
   error, no-permission — either handled or `n/a` with a reason.
7. **States are reachable and leaveable.** Every state in an entity's list
   appears in its transitions table. No orphans, no dead ends unless intended
   and noted.
8. **Journey steps are supported.** Every behavioral step in `06-journeys.md`
   names a screen that exists, an action whose verdict is `yes`, and a screen
   that actually hosts that action. A journey describing behavior the model
   does not support is a contradiction, not a nice-to-have.
9. **Scope agrees in both directions.** Nothing marked `no` or `n/a` in the
   Product capabilities table, and nothing listed under Out of scope in
   `01-product.md`, may appear as supported behavior in `02-model.md`,
   `03-screens.md`, `06-journeys.md`, or any slice. If the product genuinely
   needs it, update `01-product.md` first and record the change in
   `decisions.md`. This is the top-level form of the same failure: a decision
   made once and quietly reversed downstream.
10. **Reverse coverage.** Every entity action, screen, domain rule, and journey
    is named in some slice's `covers:` block. Anything defined and never
    covered was planned and never built. Something deliberately
    documentation-only must say so where it is defined. Field obligations are
    not checked here — check 2 owns them, and `covers:` deliberately has no
    `fields:` key.
11. **Task coverage.** Every action, screen, rule, and journey in a slice's
    `covers:` block is referenced by at least one task's `refs:` in that slice,
    unless explicitly marked slice-level or infrastructure-only.
12. **No orphan task references.** Every action, screen, rule, and journey a
    task references belongs to that slice's `covers:` block or to a slice it
    declares in `depends_on`. Field and decision references (`E-Task.priority`,
    `D-005`) are supporting context, not scoped this way, but must still
    resolve under check 13.
13. **Every stable ID resolves.** Each `E-`, `S-`, `DR-`, `J-`, `SL-`, and `D-`
    reference — in slices, journeys, tasks, `depends_on`, `Covered by`,
    relationships, and the permission matrix — points at something that exists.
14. **Every capability is accounted for.** Each row marked `yes` in the Product
    capabilities table in `01-product.md` names at least one existing screen,
    entity, rule, or slice in its `Covered by` column. A leftover `pending`
    means the capability was considered and then lost. Rows marked `no` or
    `n/a` need a reason, not a `decisions.md` entry.
15. **Permissions are complete and consistent.** If `05-constraints.md` has an
    authorization matrix, every action marked `yes` in `02-model.md` has a
    verdict for every role, no action marked `no` or `n/a` appears in the
    matrix at all, and every screen's `no-permission` state agrees with it.

**A reference is not coverage.** An ID in a task's `refs:` counts only when
that task's scope or acceptance criteria materially implement or verify the
referenced behavior. Sprinkling IDs to satisfy checks 10 and 11 produces paper
traceability, which is worse than none — it reports green while the behavior is
absent.

---

## Expectation pass

Run this when adding an entity, a screen, or a capability during
implementation. New things arrive without having been interviewed, which is
exactly when ordinary behavior goes missing.

- A new entity gets a verdict on every row of the entity action table, using
  the same table shape as the existing entities in `02-model.md`.
- A new screen addresses every concern the existing screen blocks address —
  states, affordances, entry points, back destination.
- A new capability at product level gets a row in the Product capabilities
  table in `01-product.md`, including its `Covered by` reference.

Copy the shape from what is already in the files. Every verdict is `yes`,
`no + reason`, or `n/a + reason` — an unreasoned verdict is the same as a
blank.

---

## Reporting

Report holes as a flat list naming the file and the specific gap. Do not
summarize as "mostly complete."

Each hole ends in one of two places: a fix in the planning files, or a line in
`decisions.md` recording why it is acceptable. A hole that is neither fixed nor
recorded gets rediscovered after the code is written, which is the expensive
time to find it.

---

## What this cannot do

These checks prove coverage, never quality. They can show that an action
exists, is exposed on a screen, and is covered by a task. They cannot show
that the flow is discoverable or that the screen matches what the user
imagined. That is what the review checklist at the bottom of each slice file is
for, and it is filled in by a human after using the software.
