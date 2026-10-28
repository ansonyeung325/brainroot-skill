---
name: helicopter
description: Run a project as small, user-approved tasks — every step tracked in a checklist, explained before it starts, and proven with evidence after it ends.
disable-model-invocation: true
---

# Helicopter

When the user's prompt invokes `/helicopter`, the content of your first response starts with exactly `~helicopter ~helicopter`. Shape that response the way the user's output style requires; the line is content inside that shape. Then do the work.

The user flies above the work and sees every move. You plan, explain, build, and prove — one small task at a time — and the user approves every decision.

Two leading words run this skill:

- **Gate**: a point where you stop and wait for the user's explicit "yes". You never pass a gate on your own.
- **Evidence**: something the user can check themselves — command output, a test result, a screenshot, a file path and line. A claim without evidence is not done.

## Inputs

Before step 1, ask the user for both and wait for the answers:

- **Project location**: the repo or folder you will work in.
- **Overview location**: the folder where `<project-name>-overview.html` goes.

## Steps

### 1. Hear the outline

Ask the user for the project outline. Then read the project: existing code, docs, conventions, and tools that already solve part of the problem.

Done when you can say back the goal in one sentence and the user agrees with it.

### 2. Split into tasks

Split the project into small tasks. Each task has one goal, one way to verify it, and fits in one review.

Done when every part of the outline belongs to exactly one task. Show the list to the user. **Gate.**

### 3. Fill the checklist

Write every task into the session checklist (TaskCreate). The user watches progress there.

Done when the checklist matches the approved task list one-to-one.

### 4. Write the overview

Use the `junior-change-doc` skill to write `<project-name>-overview.html` at the overview location.

- **New project (from zero)**: the work outline, then every task.
- **Change to an existing project**: the current state first (architecture, design, business logic, important rules, and everything `junior-change-doc` covers), then every task.

If `junior-change-doc` is not installed, write the same content as a plain HTML page.

Done when every checklist task has its own section in the overview.

### 5–9. Task loop

Repeat steps 5 to 9 for each task, in checklist order. Mark the task in_progress at step 5 and completed at step 9.

#### 5. Grill

Use the `mattpocock-skills:grilling` skill to interview the user about this task. If it is not installed, ask the questions yourself, one decision at a time.

Done when every decision inside the task has an answer from the user, and none is your guess.

#### 6. Explain the plan

Before you change anything, tell the user:

- **What** you will do — files, functions, commands.
- **Why** — the reason, and the existing convention or tool you reuse.
- **Impact** — what behaviour changes, what could break, what is hard to undo.

Use a concrete example for each point. **Gate.**

#### 7. Build and verify

Implement exactly the approved plan. Then verify with real evidence: run the tests, run the app, query the output.

When you cannot verify a part yourself (no permission, no device, no environment), stop and ask the user to verify it. Give the exact steps and the exact result they should see.

Done when every claim about the task has evidence attached.

#### 8. Report

Tell the user:

- What you finished.
- What changed as a result.
- How you verified it, with the evidence.

#### 9. Update

Mark the task completed in the checklist. Update the overview HTML so it shows the current state.

Done when the checklist and the overview both match reality.

## Rules

- **Unplanned work goes through a gate.** When you find extra work during any step (a bug, a missing dependency, a needed refactor), add it to the checklist, explain it to the user with the same What / Why / Impact as step 6, and wait. Do the extra work only after the user approves it.
- **Stay inside the plan.** Every decision you make traces back to an approved plan or a user answer. A new decision is a new gate.
- **Look before you propose.** Before every proposal, check for an existing pattern, convention, or tool in the project. Reuse it, unless it does not fit — then say why.
- **Explain for a beginner.** Assume the user does not know the topic. Give details and an example.
- **Hard guardrails** — leave these to the user:
  - Git push: the user pushes.
  - Databases: the user runs every query. Write the query out for them.
  - Environment files (`.env` and similar): the user edits them. Give the exact line to change.
