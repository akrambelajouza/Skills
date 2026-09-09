# Product

## What it is

Two or three sentences. What the product does, in plain language, without
feature lists.

## Who it is for

The primary user and the situation they are in when they reach for this. One
paragraph. If there is a clear secondary user, name them; if not, say so.

## The problem

What is bad today, and what "solved" looks like. Concrete enough that a
decision can be tested against it later.

## Principles

Decisions that stay consistent across the whole product. Three to six, no
more — a long list is not a set of principles.

- Example: defaults over configuration; the product should be useful before
  any setting is touched.
- Example: nothing is destructive without an undo.

## Out of scope

Explicitly not building, in this version. Push for at least three.

This section does more work than it looks like it does — it is what stops the
data model from growing indefinitely in the next phase.

- ...
- ...
- ...

## Product capabilities

Every row of the product capability checklist, with a verdict. Most will be
`no` or `n/a` — that is expected. The value is that each was considered.

Bundled capabilities take one row per sub-capability. A parent answered `yes`
with no children is a hole — that is how sign-out or password recovery goes
missing after authentication was "considered".

| Capability | Verdict | Reason | Covered by |
|---|---|---|---|
| Authentication / sign-in | yes | email and password | S-SignIn |
| Authentication / sign-out | yes | user menu | S-Settings |
| Authentication / password recovery | yes | emailed reset link | S-ResetPassword |
| Authentication / session expiry | yes | 30 days, then redirect to sign-in | S-SignIn, DR-030 |
| Account management / change email | yes | requires re-confirmation | S-Account |
| Account management / change password | yes | requires current password | S-Account |
| Account management / change name | n/a | no display name in a single-user product | — |
| Settings / preferences | yes | week start day, default view | S-Settings |
| Onboarding / first run | no | empty states carry the guidance instead | — |
| Roles and permissions | n/a | single-user product | — |
| Notifications | no | deferred to v2 | D-004 |
| Data import | no | no existing tool to import from | — |
| Data export | no | deferred | D-005 |
| Account deletion / delete account | yes | from settings, confirmed | S-Account, DR-031 |
| Account deletion / delete data | yes | everything, immediately | DR-031 |
| Account deletion / retention | yes | nothing retained after deletion | DR-031 |
| Billing / subscription | n/a | free product | — |
| Integrations | no | none in v1 | — |
| Admin functionality | n/a | no other users' data exists | — |
| Localization / language | no | English only in v1 | — |
| Localization / timezone | yes | set per account, affects due dates | E-Account.timezone |
| Localization / date format | n/a | browser locale is used; no formatting behavior of our own | — |
| Help and support | no | scope is small enough to be self-evident | — |

`Covered by` is what stops a capability from being considered and then quietly
lost. Write `pending` during phase 1 — the entities and screens do not exist
yet — and fill it in during phases 2 and 3.

The rule per verdict:

- `yes` → `Covered by` is mandatory and must name something that exists.
- `no` → a reason is mandatory. Add a `D-nnn` reference only when it is a real
  product decision or a deliberate deferral. Most rejections need no log entry,
  and forcing one produces ten meaningless decisions in phase 1.
- `n/a` → a reason is mandatory.

This extends traceability one level up: capability → entity/screen/rule →
slice → task → code.

## Glossary

Terms used consistently in every other document and in the code. Fix the
wording here so "task", "item", and "todo" do not all appear later.

| Term | Means |
|---|---|
| Task | ... |
