# Domain rules

Business logic and invariants, numbered and stable.

Domain rules are authoritative for business invariants: a screen block, slice
spec, or task contradicting a rule here loses. They do not outrank product
scope or constraints — `01-product.md`, `04-rules.md`, and `05-constraints.md`
are three mandatory dimensions, none above the others. A conflict among those
three is a planning conflict and must be resolved explicitly in the files, not
decided in passing during implementation.

Keep rules out of screens and slices. A rule stated in three places will be
changed in one.

**DR-001** — A task must always have a title of at least one non-whitespace
character.

**DR-002** — A task's due date may be in the past; overdue is a display
concern, not a validation error.

**DR-012** — A completed task can be reopened within 7 days. After that it
can only be duplicated.

The example below shows one slice's worth of rules plus a few from elsewhere
in the product. In a real package every rule is covered by some slice — check
10 enforces that, and rules are the easiest thing to write carefully and never
implement.

**DR-021** — A time block must fall within the week it is dropped into.

**DR-022** — Two time blocks for the same account may not overlap.

**DR-030** — A session expires after 30 days of inactivity.

**DR-031** — Deleting an account deletes all of its data immediately, with
nothing retained.

Each rule should be checkable — a reader can tell whether a given state
satisfies it. "The app should feel fast" is a constraint, not a rule; it
belongs in `05-constraints.md`.
