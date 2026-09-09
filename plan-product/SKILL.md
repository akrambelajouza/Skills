---
name: plan-product
description: Run a structured planning interview that turns a rough product idea into a complete, implementation-ready planning package (product definition, data model, screens, rules, journeys, and vertical slices) for an AI coding agent to build from. Use this whenever the user is starting a new app, website, tool, or product and wants to plan it before coding — including phrasings like "I want to build X", "help me plan this app", "write a PRD", "spec out this project", "set up planning docs", or when they are about to hand a project to Claude Code, Codex, or another coding agent. Also use it when a user complains that AI-built software came out with obvious basic things missing. Prefer this skill over writing an ad-hoc PRD or spec document.
---

# Plan Product

Turn a rough idea into a planning package complete enough that a coding agent
does not omit obvious product behavior.

The failure this exists to prevent is not complex features going wrong. It is
basic ones going missing — an entity with no edit affordance, a screen with no
empty state, a delete with no confirmation. Those disappear because prose
cannot be checked for omissions. Tables can.

## What this produces

```
AGENT.md                  instructions for the coding agent
planning/
├── 01-product.md         what it is, who for, out of scope, glossary
├── 02-model.md           entities: fields, editability, actions, states
├── 03-screens.md         screens: actions, edit fields, states, navigation
├── 04-rules.md           business invariants (DR-001…)
├── 05-constraints.md     auth, performance, browsers, accessibility
├── 06-journeys.md        multi-entity flows (J-001…)
├── 07-completeness.md    the checks, copied in so the package stands alone
└── slices/
    └── SL-001-<name>.md  spec + tasks + acceptance + review, one file
decisions.md              append-only log of gaps and choices
```

Templates for every one of these are in `assets/templates/`. They stay in the
skill. Use each one's structure when writing the corresponding file, but do
not copy them into the user's project ahead of time — they carry a worked
task-manager example, and example entities sitting in a file that is supposed
to become the source of truth will be mistaken for the user's own.

Two exceptions, both copied verbatim at phase 7 because their content is fixed
and not product-specific: `AGENT.md` and `planning/07-completeness.md`.

Do not invent structure. The fixed columns and headings are what make a blank
cell visible.

The templates carry a worked task-manager example. It shows the shape of each
file and the level of detail expected; it is not a complete product, and
several entities and screens it references are omitted for length. Copy the
structure, not the content.

## Two rules that decide whether this works

**Propose, never ask open-ended.** "What fields does a Task have?" gets a shrug
and a short answer. "Here's what I'd expect — title, description, priority, due
date, status, created_at. What's missing, what's wrong?" gets a correction.
Corrections are faster and more accurate than recall, and the user cannot fail
to consider something that is already on the page. Apply this to fields,
actions, screens, states, and journeys alike.

**Write progressively, one file per phase.** Do not hold everything in
conversation and write at the end. A long interview will drift from what the
user actually said, and — more importantly — the completeness passes read
tables, not transcripts. Each written file becomes the input to the next phase.

## Phases

Before the first question, create only the empty scaffolding:

```
planning/
planning/slices/
decisions.md
```

Each planning file is written at the end of its own phase, from the template's
structure but with the user's content. Nothing example-filled ever lands in
the project. The user still has something real on disk if they stop early —
whatever phases they completed.

After each phase: write the file, show what was written, ask "anything wrong
here?", and **stop and wait**. Do not chain phases in one turn. On a small
product a gate takes a minute — the point is that the review is not optional,
not that it is long.

### Phase 1 — Product

Ask what it is and who it is for. Then propose, for confirmation: the core
problem, the primary user, three to five things explicitly **out of scope**,
and the terms that will be used consistently.

Out-of-scope matters more than users expect. It is what stops the model from
growing indefinitely in phase 2. Push for it.

Then run the **product capability checklist** in
`references/expected-actions.md`. This is the only pass that can catch a whole
capability nobody named — a settings screen, account deletion, timezone
handling. Every later check works by interrogating things that already exist,
and a capability with no entity and no screen behind it leaves no blank cell to
notice.

Propose verdicts for the whole table, then confirm the uncertain ones. Most
will be `no` or `n/a`; the value is the recorded reason.

Write `01-product.md`.

### Phase 2 — Model

The longest phase and where the interviewing earns its keep.

Propose the entity list from what the user described. For each entity, in
order:

1. **Fields.** Propose the obvious set, then fill the table — including the
   `On create`, `Editable`, and `In list` columns. Those three catch a field
   that cannot be set at creation, cannot be edited, or never appears in the
   list it was meant for. Each creates an obligation on a screen in phase 3.
2. **Actions.** Read `references/expected-actions.md` and evaluate every row
   of the fixed list. Every action gets `yes`, `no + reason`, or
   `n/a + reason`. An unreasoned `no` is indistinguishable from never having
   thought about it, which is the exact failure this pass exists to catch.
3. **Relationships.** What it belongs to, what belongs to it, cardinality, and
   what happens when the other side is deleted. An unstated delete behavior is
   where an agent invents a cascade or silently orphans records.
4. **States and transitions**, if the entity has a lifecycle.

Evaluate every row, but do not interview every row. Twenty questions per
entity across eight entities is 160 exchanges, and the user will start
answering carelessly, which defeats the purpose. Propose the filled table in
one go, then ask only about the rows you are genuinely unsure of: "Three worth
confirming — permanent delete, duplicate, manual reordering. Here's what I'd
suggest and why."

The proposal is for the conversation. The written file only ever contains the
three verdicts, never "probably yes" or a hedge. An uncertain row gets asked,
not written down uncertain.

Do not generate the action list from the project context. Use the fixed list
verbatim. An agent asked to think of expected actions for a project it has
already framed narrowly will produce a narrow list.

Write `02-model.md`.

### Phase 3 — Screens

Derive screens from the model rather than inventing them fresh: every action
in `02-model.md` needs somewhere to live. Propose the screen list, let the user
correct it, then for each screen fill the template block: actions hosted, the
three field lists (create, edit, display), the five states with
`n/a + reason`, the affordances, where the user arrives from, and where back
goes. All three field lists are required on every screen — `none` or
`n/a + reason` rather than omitted, since an absent list looks exactly like a
forgotten one.

Then run the screen affordance checklist in
`references/expected-actions.md`. The entity pass finds missing actions; this
one finds missing confirmations, empty states, and post-save destinations.

Write `03-screens.md`.

### Phase 4 — Rules and constraints

Mostly harvesting: several things the user said in earlier phases were
invariants in disguise ("a completed task can't be reopened after a week").
Propose those back as numbered rules and ask what is missing.

Constraints should be short — only what actually changes code. Auth model,
performance budgets that matter, browser and device support, accessibility
target, data retention.

If the product has more than one role, expand the authorization section into a
role × entity × action grid. Permissions written as prose get implemented
inconsistently, and a missing cell is invisible in a sentence. If it is
single-user, write `Authorization matrix: n/a — single-user application` and
move on. This is conditional, not a required artifact.

Write `04-rules.md` and `05-constraints.md`.

### Phase 5 — Journeys

Multi-step flows crossing screens, entities, or meaningful states. If a journey
maps one-to-one to a single action, it is not a journey — it is a row in the
model, and duplicating it here creates drift. Include the failure paths, not
just the happy one.

Every behavioral step names the screen and the action ID it uses. Without that,
a journey can describe something the model does not support and nothing will
catch it.

Write `06-journeys.md`.

### Phase 6 — Slices

Group the work into vertical slices that each deliver usable value and can be
built and reviewed independently. Each slice file holds its spec, its tasks,
its acceptance criteria, and its review checklist together — one file, so the
task and the spec cannot drift apart.

Tasks reference IDs (`E-Task.edit`, `S-TaskDetail`, `DR-012`, `J-007`) rather
than restating content. Ordering lives in each slice's `depends_on` field, not
in a separate roadmap that goes stale.

Write `slices/SL-001-*.md` and onward.

### Phase 7 — Completeness review

Read `references/completeness-passes.md` and run both passes — expectation
first, then consistency. Report the holes
as a list. Each hole either becomes a fix in the files or a line in
`decisions.md` explaining why it is fine. Do not close this phase with holes
unresolved and unrecorded.

Then copy two files verbatim into the project: `assets/templates/AGENT.md` to
the repository root, and `assets/templates/07-completeness.md` into
`planning/`. Both are fixed and not product-specific, which is why they are
copied rather than written. The coding agent re-runs the consistency pass
after every task and has no access to this skill's `references/` directory —
the finished package has to stand alone.

## Backtracking is normal

Phase 5 will reveal that an entity was wrong. Go back and fix `02-model.md`.
Returning to an earlier file mid-interview is expected behavior, not a failure
— do not push forward and log the contradiction in `decisions.md` instead of
simply fixing it. `decisions.md` is for choices and gaps, not for known-wrong
specs.

## What this cannot do

The passes prove coverage, never quality. They can show that an edit action
exists, is exposed on a screen, and is covered by a task. They cannot show that
the edit flow is discoverable or that the screen matches what the user
imagined. That judgment stays with the user, at the review checklist at the
bottom of each slice file, after actually using the thing.

Say this plainly when handing over the package. A user who believes the
checklist proves correctness will skip the looking, which is the failure that
started this.

## Handoff

When the package is done, tell the user what to do next: point their coding
agent at `AGENT.md`, and build one slice at a time, running the slice's review
checklist before starting the next.
