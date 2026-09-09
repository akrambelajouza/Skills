# User journeys

Multi-step flows crossing screens, entities, or meaningful states. A journey
that maps to a single action is not a journey — it is a row in `02-model.md`,
and repeating it here creates drift. Delete it.

Every behavioral step names the screen it happens on and the action ID it uses,
as `[S-Screen, E-Entity.action]`. Both must exist, the action's verdict must be
`yes`, and that screen must host it. Prose steps without IDs are how a journey
comes to describe behavior the product does not support.

---

## J-001 — Capture and finish a task

**Trigger:** Something comes up that the user must not forget.
**Entities:** E-Task
**Screens:** S-TaskList, S-TaskDetail
**Outcome:** The task is captured, worked, and cleared from the active list.

**Happy path**

1. Quick-creates with just a title — `[S-TaskList, E-Task.create, DR-001]`
2. Opens it to add a due date and priority — `[S-TaskDetail,
   E-Task.view_detail]`, then `[S-TaskDetail, E-Task.edit]`
3. Marks it complete — `[S-TaskList, E-Task.status_change]`
4. Archives it — `[S-TaskList, E-Task.archive]`

Every behavioral step names the screen and the action it uses. Without the
action ID, a journey can describe something the model does not support and no
check will notice — "the user duplicates the task" reads fine even when
`E-Task.duplicate` is `no`.

**Alternative paths**

- Reopens a completed task within the window DR-012 allows —
  `[S-TaskDetail, E-Task.status_change, DR-012]`
- Archives by mistake and undoes from the toast — `[S-TaskList, E-Task.undo]`

**Failure paths**

- Saves with an empty title: rejected per DR-001, inline error, focus stays.
- Reopen attempted after the DR-012 window: refused, with duplicate offered
  as the alternative.

This crosses two screens and several states, which is what makes it a journey.
A flow that maps one-to-one to a single action is not one — it is a row in
`02-model.md`, and repeating it here creates drift.

---

## J-002 — ...

