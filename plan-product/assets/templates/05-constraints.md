# Constraints

Only what actually changes implementation decisions. This is deliberately not
a full non-functional requirements document — a long one gets skipped.

## Authentication and authorization

Who signs in, how, and what happens on session expiry. If single-user with no
auth, say that explicitly.

### Authorization matrix

Required only when the product has more than one role. For a single-role
product, write one line and move on:

> Authorization matrix: n/a — single-user application.

When there are multiple roles, enumerate them. Permissions stated in prose get
implemented inconsistently; a grid makes a missing cell visible the same way
the entity table does.

One row per role × entity × action. A fixed column set cannot work here: it
would silently drop archive, restore, duplicate, status change, export, and
assign — the actions where permission mistakes actually happen.

This example assumes a multi-role variant of the product with an `E-Project`
entity and both actions supported in `02-model.md`. Adapt the IDs to yours.

| Role | Action | Allowed | Scope / reason |
|---|---|---|---|
| Owner | `E-Project.create` | yes | |
| Owner | `E-Project.edit` | yes | |
| Owner | `E-Project.delete` | yes | |
| Owner | `E-Project.assign` | yes | manages membership |
| Member | `E-Project.view_list` | yes | assigned projects only |
| Member | `E-Project.edit` | no | owner only |
| Member | `E-Task.create` | yes | within assigned projects |
| Member | `E-Task.edit` | yes | own tasks only |
| Member | `E-Task.archive` | yes | own tasks only |
| Member | `E-Task.restore` | no | owner only |

Every action marked `yes` in `02-model.md` gets a verdict for every role. A
missing row is a hole, not an implied no — that assumption is how a role
silently gains or loses access.

The matrix covers only actions the model supports. An action marked `no` or
`n/a` in `02-model.md` must not appear here at all: a row saying a role may not
do it implies the capability exists and is merely restricted, which is a
different product than one where it does not exist.

Each screen's `no-permission` state in `03-screens.md` must be consistent with
this grid.

## Performance

Budgets that matter, with numbers. "Task list renders in under 200ms for 1000
tasks" is a constraint; "should be fast" is not.

## Platforms

Browsers and versions, minimum screen width, mobile or desktop, offline
behavior.

## Accessibility

Target level, keyboard navigation expectations, contrast requirements.

## Data

Where it lives, what is retained, what is deleted and when, backup
expectations, anything with a privacy or regulatory constraint.

## Observability

What is logged, what errors surface to the user versus to a log.
