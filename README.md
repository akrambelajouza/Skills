# Skills

This is a growing collection of skills that help AI coding agents understand
software projects more clearly.

The first skill helps you think through a product before building it. The
second helps you understand a project that already exists. Both came from the
same problem: important details are easy to miss when information is spread
across a long conversation or a large codebase.

## Plan Product

[Plan Product](plan-product/) turns a rough product idea into a thorough,
implementation-ready plan.

Long planning conversations can feel productive while still leaving basic
questions unanswered. Can an item be edited? What happens when a list is
empty? Does deleting one record affect another? Where does the user go after
saving? These gaps often become missing or inconsistent features during
implementation.

Plan Product avoids this by guiding you through the product in clear phases.
Instead of asking broad questions, it proposes likely answers for you to
review and correct. It covers:

- the product, its users, and what is out of scope
- entities, fields, relationships, actions, and states
- screens, navigation, empty states, errors, and permissions
- business rules, technical constraints, and user journeys
- small vertical slices that can be built and reviewed one at a time
- a final completeness check that connects the whole plan

The skill writes each part of the plan as it is agreed, so progress is not
lost during a long session. The finished package gives a coding agent a clear
source of truth and gives you specific checkpoints to review.

Try it with a prompt such as:

> Use Plan Product to help me plan a personal finance app before we start
> coding.

## Project Tour

[Project Tour](project-tour/) creates a conversational walkthrough of an
existing codebase.

Starting work on an unfamiliar project usually means jumping between folders,
configuration files, documentation, tests, and entry points before the system
makes sense. Project Tour does that investigation for you, then explains what
it found as if a teammate were sitting beside you and walking you through the
project.

It reads the implementation, configuration, tests, and documentation; finds
the ways the project starts; and follows a real journey through the system.
It then creates a `PROJECT_TOUR.md` that explains:

- the main mental model
- how the project starts and runs
- one end-to-end journey through the code
- the responsibilities of the main components
- state, data, external services, and configuration
- where to make common changes
- how to test and debug the project
- anything uncertain, missing, or inconsistent in the available evidence

The result is written for listening as much as reading: connected,
plain-language explanations take priority over long file inventories. The
skill also separates verified facts from reasonable inferences and never
reads secret values into the tour.

Try it with a prompt such as:

> Use Project Tour to explain how this codebase works. Make it easy to listen
> to.

## Using the skills

Each skill is self-contained in its own directory. Add the directory to the
skills location used by your AI coding agent, then ask the agent to use the
skill by name. The exact installation location depends on the agent you use.

You can also open each skill's `SKILL.md` to see its full behavior, workflow,
and safety rules.

## What is next

More skills will be added over time. The goal is to keep each one focused on a
real, recurring problem and make it useful without requiring a complicated
setup.
