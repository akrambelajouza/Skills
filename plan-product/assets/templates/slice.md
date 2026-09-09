---
id: SL-001
name: task-management
depends_on: []
covers:
  entities: [E-Task]
  actions: [E-Task.create, E-Task.edit, E-Task.archive, E-Task.restore,
            E-Task.undo, E-Task.view_list, E-Task.view_detail]
  screens: [S-TaskList, S-TaskDetail]
  rules: [DR-001, DR-012]
  journeys: [J-001]
---

# SL-001 — Task management

## Goal

One paragraph: what the user can do once this slice ships that they could not
do before. If it does not describe usable value on its own, it is not a slice.

## Spec

Behavior specific to this slice — what happens, in what order, with what
feedback. Do not restate content that lives in `02-model.md`, `03-screens.md`,
or `04-rules.md`; reference the ID instead. Duplication here is how the plan
starts disagreeing with itself.

Edge cases and failure behavior belong in this section, not in a separate
document.

## Tasks

### T-01 — Task list and detail views

**refs:** `E-Task.view_list`, `E-Task.view_detail`, `S-TaskList`, `S-TaskDetail`
**scope:** read-only rendering of both screens, including their display fields
and all five states

### T-02 — Create task

**refs:** `E-Task.create`, `S-TaskList`, `DR-001`
**scope:** inline create on the list, title required, other fields defaulted

### T-03 — Edit task

**refs:** `E-Task.edit`, `S-TaskDetail`, `E-Task.priority`, `DR-012`, `J-001`
**scope:** every field marked `Editable: yes` on E-Task, save, cancel,
validation

### T-04 — Archive, restore, undo

**refs:** `E-Task.archive`, `E-Task.restore`, `E-Task.undo`, `S-TaskList`,
`S-TaskDetail`
**scope:** archive with undo toast, restore from the archived filter

Tasks reference IDs and never restate their content. Anything the agent needs,
it resolves by following the reference.

Every ID in the `covers:` block above must appear in some task's `refs:` here.
In the other direction, every action, screen, rule, and journey a task
references must belong to `covers:` or to a declared dependency. Field and
decision references — `E-Task.priority` or a `D-` entry — are supporting context and
are not scoped this way, but must still resolve to real definitions.

Both directions are checked. A slice covering something no task implements
recreates the original failure one level down, and a reference counts only when
the task's scope or acceptance criteria actually implement or verify it.

## Acceptance criteria

Written so each can be verified as done or not done. No "works correctly".

- [ ] A task can be created from S-TaskList with only a title.
- [ ] S-TaskDetail pre-fills every editable field with its current value.
- [ ] Every field marked `Editable: yes` on E-Task can be changed and persists.
- [ ] Invalid values show an inline error and do not persist.
- [ ] Cancel leaves the record unchanged.
- [ ] Archiving shows an undo affordance; undo restores the task.
- [ ] S-TaskList empty state appears with zero tasks and offers create.
- [ ] DR-012 is enforced and its violation message is shown to the user.

## Tests

- Every action in `covers.actions` has automated coverage at whichever level
  actually exercises it — unit, integration, or end-to-end
- Validation and rule enforcement: integration
- J-001: end-to-end, happy path and its failure branch
- Regression: previously closed slices still pass

## Review — filled in by the human, after using it

Mechanical coverage does not prove the product is right. Open the running
software and answer these. The agent does not fill this in.

- [ ] Ran J-001 start to finish in the actual UI.
- [ ] Every action I expected to find, I found — without being told where.
- [ ] Nothing I tried to do felt blocked or missing.
- [ ] Empty, error, and loading states look deliberate, not accidental.
- [ ] Wording and layout match the conventions used in earlier slices.

**Gaps found:**

Each one becomes a new task in this slice or a line in `decisions.md`. The
slice does not close with an unrecorded gap.
