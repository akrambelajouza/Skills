# Expected actions and affordances

Three fixed checklists — product, entity, and screen. Use them verbatim. Their value comes from being fixed —
a list generated per project will be shortened by whatever framing the project
already has, which is how ordinary capabilities go unconsidered.

Every item gets one of three verdicts:

- `yes` — supported
- `no + reason` — deliberately excluded, reason recorded
- `n/a + reason` — meaningless for this entity, reason recorded

An unreasoned `no` is indistinguishable from never having thought about it.
Require the reason.

---

## Product capability checklist

Evaluate every row once, in phase 1, before any entity exists.

The entity and screen checklists can only interrogate things already on the
page. They cannot find a Settings screen nobody thought of, or account deletion
nobody considered — there is no row to leave blank. This checklist is the one
that catches whole capabilities going missing.

Several rows are bundles. A single `yes` on "Authentication" can hide a missing
sign-out or password reset — the same considered-but-incomplete failure, one
level down. **A bundled capability cannot take a bare `yes`: every
sub-capability listed below gets its own verdict row.**

| Capability | Sub-capabilities each needing a verdict | What to ask |
|---|---|---|
| Authentication | sign-in, sign-out, password recovery, session expiry | How does someone get in, get out, and get back in after forgetting? |
| Account management | change email, change password, change name/profile | What about their own account can the user alter? |
| Settings / preferences | — | Is anything configurable, and where does it live? |
| Onboarding / first run | — | Is a brand-new account different from an empty one? |
| Roles and permissions | role list, assignment, revocation | Is there more than one kind of user? |
| Notifications | channels, triggers, opt-out | Does the product ever need to reach the user? How? |
| Data import | — | Does the user arrive with data from somewhere else? |
| Data export | — | Can the user get their data out? |
| Account / data deletion | delete account, delete data, retention after deletion | Can a user leave, and what happens to their data? |
| Billing / subscription | plan selection, payment method, invoices, cancellation | Is there money involved, now or later? |
| Integrations | — | Does this need to talk to anything external? |
| Admin functionality | — | Does anyone need to see or fix other users' data? |
| Localization / timezone | language, timezone, date and number format | Multiple languages? Users in different timezones? |
| Help and support | — | How does a stuck user get unstuck? |

Rows with `—` are single questions and take one verdict. Rows with
sub-capabilities take one row each, written `Parent / sub-capability`. A parent
answered `no` or `n/a` needs no children — the reason covers them.

Most products answer `no` or `n/a` to most of these. That is the expected
outcome — the value is the reason attached, not the yes.

Record the verdicts in the Product capabilities table in `01-product.md`.
Anything answered `yes` must map to one or more concrete entities, screens,
rules, or slices in later phases — that is what the `Covered by` column
records.

---

## Entity action checklist

Evaluate every row once per entity, in phase 2. Evaluate all of them; do not
ask about all of them. Propose the filled table, then ask only about the rows
you are genuinely unsure of — twenty sequential questions per entity produces
careless answers, which defeats the purpose.

The `Key` column is the stable identifier. Every reference elsewhere —
screens, journeys, slices, tasks, permissions — uses `E-<Entity>.<key>`, never
the display name. `E-Task.view_list` is a usable ID; `E-Task.View — list` is
not.

| Action | Key | What to ask |
|---|---|---|
| Create | `create` | How does one come into existence? Manually, imported, auto-generated? |
| View — list | `view_list` | Where does the user see many of these at once? |
| View — detail | `view_detail` | Is there a single-record view, or is the list the only view? |
| Edit | `edit` | Which fields? All of them, or a subset? |
| Delete | `delete` | Hard delete, or is history required? |
| Archive / hide | `archive` | Is there a state between active and deleted? |
| Restore | `restore` | If archived or deleted, can it come back? |
| Duplicate | `duplicate` | Is copying an existing one a plausible shortcut? |
| Rename | `rename` | Does it have a name distinct from full editing? |
| Reorder / move | `reorder` | Manual ordering, or moving between parents? |
| Bulk select | `bulk_select` | Can the user act on several at once? Which actions? |
| Search / filter | `search_filter` | How does the user find one among many? |
| Sort | `sort` | Which fields, and what is the default? |
| Status change | `status_change` | Are there transitions distinct from editing fields? |
| Assign / share | `assign` | Does it belong to someone? Can that change? |
| Export | `export` | Does the user ever need this data outside the product? |
| Undo | `undo` | Is any action destructive enough to need reversal? |
| History / audit | `history` | Does the user need to know what changed and when? |
| Comment / note | `comment` | Is there discussion or annotation attached to it? |
| Attach files | `attach_files` | Does it carry documents or images? |

Not every entity needs most of these. The point is not to say yes — it is that
each one was considered and the answer recorded.

---

## Screen affordance checklist

Evaluate every row once per screen, in phase 3, the same way: propose, then
ask about the uncertain rows only.

| Affordance | What to ask |
|---|---|
| Empty state | What is shown with no data, and what is the call to action? |
| Loading state | What is shown while data is in flight? |
| Error state | What is shown when the load or save fails, and can the user retry? |
| No-permission state | What does someone without access see — hidden, or a message? |
| First-run state | Is the first visit different from an ordinary empty state? |
| Back navigation | Where does back go, and is it always reachable? |
| Post-save destination | After saving, does the user stay, return, or go to the new record? |
| Cancel | Can an in-progress edit be abandoned, and is unsaved work warned about? |
| Destructive confirmation | Does delete or archive ask first? Is it undoable instead? |
| Validation display | Where do field errors appear, and when — on blur or on submit? |
| Entry points | Every way a user can arrive here. More than one is common. |
| Primary action | What is the single most important thing to do on this screen? |
| Pagination / volume | What happens at 10 records? At 10,000? |
| Responsive behavior | What changes on a narrow screen, if anything? |

The entity checklist catches missing capabilities. This one catches the holes
that only appear once a capability has a home — no confirmation on delete, no
empty state on a list, no defined destination after save.
