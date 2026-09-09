# Completeness passes

Two passes, run in this order. They catch different failures and neither
substitutes for the other.

---

## Pass 1 — Expectation completeness

*Did anyone ever think of this?*

Run before the consistency pass. Cross-checking documents against each other
cannot find something that all of them forgot: if both `02-model.md` and
`03-screens.md` omit edit, they agree perfectly and the consistency pass
reports nothing. The same holds one level up — nothing cross-checks a
capability that was never named at all.

1. **Product level.** Confirm every row of the product capability checklist
   has a verdict in `01-product.md`. This runs first because it is the only
   one that can find a capability with no entity and no screen behind it.
2. **Entity level.** For every entity in `02-model.md`, confirm every row of
   the entity action checklist has a verdict. Any action with no verdict is a
   hole.
3. **Screen level.** For every screen in `03-screens.md`, confirm every
   concern in the screen affordance checklist is explicitly addressed
   somewhere in the screen block. The block does not reproduce the checklist
   row for row — states live under **States**, entry points under **Reached
   from**, the rest under **Affordances**. What matters is that each concern
   has an answer, not that it sits in a particular table.
4. Flag every `no` and `n/a` that carries no reason. An unreasoned verdict is
   the same as a blank.

This is the pass that addresses the original failure. Run it even when the
plan looks finished.

---

## Pass 2 — Specification consistency

*Do the documents agree with each other?*

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

## Reporting

Report holes as a flat list, grouped by pass, each naming the file and the
specific gap. Do not summarize as "mostly complete."

Each hole ends in one of two places:

- a fix in the planning files, or
- a line in `decisions.md` recording why it is acceptable

Nothing else. A hole that is neither fixed nor recorded will be rediscovered at
the worst moment, which is after the code is written.

---

## Re-running

Run pass 2 again at the close of every slice, not only at the end of planning.
Implementation reveals new entities and fields, and the moment those land in
`02-model.md` without a screen, the plan has quietly regressed.
