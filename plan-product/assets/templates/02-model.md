# Data model

One section per entity. Keep the columns fixed — a blank cell is only obvious
when every row has the same shape.

**ID convention.** Entities are `E-<Name>`. Actions and fields are both
referenced as `E-<Name>.<key>`, always lowercase snake_case, never the display
name:

```
E-Task.create        E-Task.view_list      E-Task.status_change   (actions)
E-Task.priority      E-Account.timezone                           (fields)
```

Screens are `S-<Name>`, rules `DR-nnn`, journeys `J-nnn`, slices `SL-nnn`,
decisions `D-nnn`. Screens, journeys, slices, tasks, and the permission matrix
all reference these IDs rather than restating content or using prose names.

---

## E-Task

One-line description of what this represents and when one is created.

### Fields

| Field | Type | Required | On create | Editable | In list | Notes |
|---|---|---|---|---|---|---|
| id | uuid | auto | auto | no | no | |
| title | string | yes | user | yes | yes | |
| description | text | no | default | yes | no | empty on quick create |
| priority | enum: low, medium, high | yes | default | yes | yes | defaults to medium |
| due_date | date | no | default | yes | yes | none on quick create |
| status | enum: see states | yes | default | via transitions | yes | not directly editable |
| created_at | datetime | auto | auto | no | no | |

`On create` takes one of four values:

- `user` — the user supplies it on a creation surface
- `default` — the system sets it, and the user may override it later
- `auto` — the system sets it and the user never touches it
- `n/a` — the field does not exist at creation time

Three columns each create an obligation on `03-screens.md`, and each is checked
in the consistency pass:

| Column | Obligation |
|---|---|
| `On create: user` | appears in some screen's **Create fields** |
| `Editable: yes` | appears in some screen's **Edit fields** |
| `In list: yes` | appears in some list screen's **Display fields** |

The `Editable` column is what catches a missing edit affordance. The other two
catch the same failure at creation and at display — a field that can be set but
never shown, or that the user is silently unable to set when the record is
made.

### Actions

Every row of the entity action checklist gets a verdict. No blanks.

| Action | Key | Verdict | Reason |
|---|---|---|---|
| Create | `create` | yes | |
| View — list | `view_list` | yes | |
| View — detail | `view_detail` | yes | |
| Edit | `edit` | yes | |
| Delete | `delete` | no | completed work is kept for history |
| Archive | `archive` | yes | |
| Restore | `restore` | yes | |
| Duplicate | `duplicate` | yes | recurring work is common |
| Rename | `rename` | n/a | no name field distinct from title; covered by edit |
| Reorder | `reorder` | yes | manual ordering within a day |
| Bulk select | `bulk_select` | yes | archive and status change only |
| Search / filter | `search_filter` | yes | by title, status, priority |
| Sort | `sort` | yes | due date default |
| Status change | `status_change` | yes | see transitions |
| Assign / share | `assign` | n/a | single-user product |
| Export | `export` | no | not in v1, see D-005 |
| Undo | `undo` | yes | archive only |
| History / audit | `history` | no | not needed for a single user |
| Comment | `comment` | n/a | collaboration is not part of this product |
| Attach files | `attach_files` | no | out of scope, see 01-product.md |

### Relationships

| Relationship | Target | Cardinality | Required | On target delete |
|---|---|---|---|---|
| account | E-Account | many-to-one | yes | delete task |
| time_blocks | E-TimeBlock | one-to-many | no | delete blocks |

Every relationship states what happens when the target goes away. "On target
delete" left blank is how an agent invents a cascade — or fails to, and orphans
records. Cardinality answers whether the UI needs a picker, a list, or neither.

### States

`open`, `completed`, `cancelled`, `archived`

### Transitions

| From | To |
|---|---|
| open | completed, cancelled, archived |
| completed | open, archived |
| cancelled | open, archived |
| archived | open |

Every state must appear in this table at least once as a `From` and once as a
`To`, unless a dead end or entry point is intended and noted.

---

## E-Account, E-TimeBlock

The full example has two more entities. They follow the identical structure —
fields table, action table with keys and verdicts, states, transitions — and
are omitted here only to keep the template short. `E-Account` holds
`timezone`; `E-TimeBlock` is what `J-002` schedules.
