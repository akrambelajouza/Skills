# Screens

One block per screen, page, or modal. Same headings every time — that is what
makes an omission visible at a glance.

Screens are derived from `02-model.md`: every action marked `yes` there needs a
home here.

---

## S-TaskList

**Entity:** E-Task
**Purpose:** Where the user sees and works through their tasks.

**Actions hosted:** `E-Task.create`, `E-Task.view_list`, `E-Task.archive`,
`E-Task.restore`, `E-Task.undo`, `E-Task.reorder`, `E-Task.bulk_select`,
`E-Task.search_filter`, `E-Task.sort`, `E-Task.status_change`

**Create fields:** `E-Task.title` (inline create; everything else defaulted)

**Edit fields:** none — inline status toggle only

**Display fields:** `E-Task.title`, `E-Task.priority`, `E-Task.due_date`,
`E-Task.status`

Actions and fields are named by their ID from `02-model.md`, never in prose.
That is what lets the consistency pass match them mechanically.

All three field lists are required on every screen. Write `none` or
`n/a + reason` rather than omitting one — an absent list is indistinguishable
from a forgotten one, which is the whole failure mode.

**States**

| State | Verdict | What the user sees |
|---|---|---|
| loading | required | skeleton rows |
| ready | required | ordered task list |
| empty | required | "No tasks yet" with a create button |
| error | required | message plus retry |
| no-permission | n/a | single-user product |

**Affordances**

| Affordance | Answer |
|---|---|
| First-run | same as empty state |
| Back navigation | n/a — this is the root screen |
| Post-save destination | stays on list, new task appears in place |
| Cancel | inline create dismisses on escape |
| Destructive confirmation | archive shows an undo toast, no dialog |
| Validation display | inline under the field, on submit |
| Primary action | create task |
| Pagination | infinite scroll past 50 |
| Responsive | single column below 640px, filters collapse into a sheet |

**Reached from:** app root, post-login
**Back goes to:** n/a

---

## S-TaskDetail

**Entity:** E-Task
**Purpose:** View and edit a single task.

**Actions hosted:** `E-Task.view_detail`, `E-Task.edit`, `E-Task.archive`,
`E-Task.duplicate`, `E-Task.status_change`

**Create fields:** n/a — records are never created from this screen

**Edit fields:** `E-Task.title`, `E-Task.description`, `E-Task.priority`,
`E-Task.due_date`

**Display fields:** all fields, including `E-Task.status` and
`E-Task.created_at`

Compare this list against `02-model.md`. Every field marked `Editable: yes`
must appear in some screen's edit fields — this is the check that catches a
missing edit affordance before any code exists.

**States**

| State | Verdict | What the user sees |
|---|---|---|
| loading | required | skeleton |
| ready | required | full record |
| empty | n/a | a detail screen always has a record |
| error | required | not-found and load-failure are distinct |
| no-permission | n/a | single-user product |

**Affordances**

| Affordance | Answer |
|---|---|
| First-run | n/a — never the first screen a new user sees |
| Back navigation | always visible, top left |
| Post-save destination | stays on detail, shows saved confirmation |
| Cancel | reverts fields, warns if changes are unsaved |
| Destructive confirmation | archive confirms, offers undo |
| Validation display | inline under each field, on blur |
| Primary action | save |
| Pagination | n/a — single record |
| Responsive | fields stack below 640px |

**Reached from:** S-TaskList (row click), S-WeekView (block click)
**Back goes to:** whichever screen the user came from

---

## S-SignIn, S-ResetPassword, S-Account, S-Settings, S-WeekView

The full example has five more screens, each an identical block. They are
omitted here for length, but every screen named in `01-product.md`'s
`Covered by` column has one — an ID that appears there and nowhere else is a
broken reference, which check 10 catches.
