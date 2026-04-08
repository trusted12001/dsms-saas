# DSMS – Read Me First

Before starting any AI session, load the Master Boot Prompt from `docs/AI_PROMPT_LIBRARY.md`.

This project uses structured AI-assisted development.

Before making any change, follow these steps.

---

## Step 1 — Understand the Project

Read these documents in order:

1. docs/AI_MEMORY.md
2. docs/AI_DEVELOPMENT_BLUEPRINT.md
3. docs/HANDOFF.md
4. docs/TASK_BOARD.md

These explain:

- what DSMS is
- how the architecture should work
- the project direction
- current priorities
- next tasks

---

## Step 2 — Identify the Current Task

From `docs/TASK_BOARD.md`, identify:

- what has been completed
- what is in progress
- what is next
- what must not be broken

Work on one verified task at a time.

---

## Step 3 — Respect the Architecture

Before changing code:

- understand the relevant module
- understand who owns the data
- check tenant impact
- check role/permission impact
- check reporting/financial impact

Never make blind structural changes.

---

## Step 4 — Use AI in Consultant Mode

When prompting AI:

- load project docs first
- explain the exact problem
- ask for minimal safe changes
- avoid broad rewrites
- ask for reasoning before major code

AI should behave like a technical consultant, not a reckless code generator.

---

## Step 5 — Test Properly

After each change, test:

- Create
- Edit
- View
- Delete / archive
- Permissions
- Tenant isolation
- Reporting correctness
- Financial correctness where applicable

---

## Step 6 — Update Documentation

If you complete meaningful work, update:

- docs/HANDOFF.md
- docs/TASK_BOARD.md

If architecture decisions change, also update:

- docs/AI_DEVELOPMENT_BLUEPRINT.md
- docs/AI_MEMORY.md
- docs/ARCHITECTURE_NOTES.md

---

## Guiding Principle

DSMS is a production-minded SaaS platform.

Always prioritize:

- stability
- data accuracy
- tenant safety
- maintainability
- incremental progress
