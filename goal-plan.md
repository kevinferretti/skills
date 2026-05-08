---
name: goal-plan
description: Create or update a self-contained goal-ready plan folder for Codex `/goal` runs. Use when the user wants a plan, charter, checklist, definition of done, end-state diagrams, constraints, final verification standards, stop conditions, or handoff folder under `./goals/` that lets them later run a terse goal such as `/goal Implement the plan in ./goals/example/GOAL_PLAN.md.`
---

# Goal Plan

## Overview

Produce a goal folder that is strong enough for a long-running Codex goal: clear objective, bounded scope, repo-grounded context, end-state diagrams, explicit stop conditions, final verification standards, and a compatibility check against other goal plans.

This skill prepares the work. Do not implement the plan unless the user explicitly asks.

## Workflow

1. Establish the goal shape.
   - Interview me relentlessly about every aspect of this plan until we reach a shared understanding. Walk down each branch of the design tree, resolving dependencies between decisions one-by-one. For each question, provide your recommended answer.
   - If the answer can be found by reading the repo, docs, issues, logs, or config, inspect those instead of asking.
   - Record assumptions in the plan when moving forward without a user answer.

2. Ground the plan.
   - Read the relevant `AGENTS.md`, README, architecture docs, existing plans, tests, source files, and deployment notes.
   - Check the working tree before editing.
   - Preserve unrelated user changes.
   - Note unresolved unknowns instead of pretending they are known.

3. Create or update one durable goal folder.
   - Prefer an obvious repo-local file that a later goal can find quickly.
   - Use the path structure `./goals/<goal-slug>/GOAL_PLAN.md`.
   - Put diagrams under `./goals/<goal-slug>/diagrams/`.
   - Keep the folder self-contained enough that a future Codex run can execute it after reading only the plan, diagrams, and listed context files.

4. Model the intended end state with diagrams.
   - Add diagrams when they clarify what the completed world should look like: workflows, infrastructure, architecture, data flow, sequence, state transitions, component boundaries, deployment topology, verification coverage, or any other shape that matters.
   - Choose the smallest diagram set that removes important ambiguity. Do not create decorative or redundant diagrams.
   - Prefer Mermaid `.mmd` files as canonical source because Codex can read and update them.
   - When local tooling is available, render diagrams to viewable SVG or PNG files under `./goals/<goal-slug>/diagrams/rendered/`.
   - If rendering cannot be done, keep the `.mmd` source and note the render gap in the plan.
   - Link every diagram from an "End State Model" section and state what question each diagram answers.
   - Treat diagrams as part of the plan contract: if implementation would drift from them, update the plan or stop and ask.

5. Define final verification standards.
   - Do not use checkpoints as the main execution structure. Let the `/goal` feature manage its own working cadence.
   - Include final verification that must pass before the goal can be called done.
   - Prefer exact commands, URLs, browser flows, test files, lint/build steps, deployment checks, and artifact inspections.
   - For every verification item, state the expected pass condition and what to report if it fails.
   - Include at least one verification item that proves the implemented result matches the End State Model when diagrams are present.

6. Check parallel-plan compatibility.
   - Before marking the plan ready, inspect other goal folders under `./goals/`, especially `GOAL_PLAN.md` files and diagrams.
   - Compare the new plan with existing goal plans for likely conflicts if run in parallel.
   - Look for overlapping files/directories, schema or migration changes, dependency/lockfile changes, shared config/env changes, test fixture changes, Docker/deployment changes, branch/git operations, generated artifacts, and competing assumptions.
   - Use diagrams to detect architectural, data-flow, infrastructure, or workflow conflicts that may not be obvious from task lists.
   - Add a "Parallel Compatibility Review" section to the plan.
   - If conflicts are minor, list coordination notes. If conflicts are massive or unsafe, leave status as `Draft` or `Blocked` and explain what must be sequenced or split before running goals in parallel.

7. Finish with a goal handoff.
   - Include a "Goal Handoff" section in the plan with the recommended `/goal` command.
   - Make the command terse, but not brittle. Prefer `/goal Implement the plan in <path>.`
   - In the final response, link the plan folder, the plan file, and the recommended `/goal` command.

## Plan Template

Use this structure unless an existing project plan format is clearly better:

````markdown
# <Goal Name> Goal Plan

Created: <YYYY-MM-DD>
Status: Draft | Ready for /goal | In progress | Complete | Blocked

## Goal Handoff

Recommended command:

```text
/goal Implement the plan in <relative-or-absolute-path-to-this-file>.
```

## Goal Folder Layout

```text
./goals/<goal-slug>/
  GOAL_PLAN.md
  diagrams/
    <diagram-name>.mmd
    rendered/
      <diagram-name>.svg
```

## Objective

<One durable outcome Codex should achieve.>

## Definition Of Done

- [ ] <Observable final condition.>
- [ ] All final verification standards pass.
- [ ] <Plan checkboxes and progress log are updated.>
- [ ] <Known risks or follow-ups are documented.>

## Scope

- [ ] <What may change.>

## Non-Goals

- [ ] <What must not change.>

## Constraints

- [ ] <Repo, branch, platform, privacy, secret-handling, compatibility, style, or deployment rule.>

## Required Context

- [ ] `<file-or-doc>` - <why Codex should read it first>

## End State Model

These diagrams define the intended completed result. If implementation choices conflict with them, update the plan or stop and ask before drifting.

- [ ] [`./diagrams/<diagram-name>.mmd`](./diagrams/<diagram-name>.mmd) - <what this diagram clarifies>
- [ ] [`./diagrams/rendered/<diagram-name>.svg`](./diagrams/rendered/<diagram-name>.svg) - <rendered view, if available>

Diagram coverage:

- [ ] <Workflow, architecture, infrastructure, data flow, sequence, state, verification map, or other relevant category> is covered or intentionally omitted because <reason>.

## Assumptions To Preserve Or Validate

- [ ] <Assumption and how to validate it, if validation is possible.>

## Stop Or Pause Conditions

- [ ] Stop and ask before <destructive action, production action, secret exposure, large scope change, unresolved product decision, or human approval>.
- [ ] Pause if <verification failure or ambiguity> means the plan itself needs revision.

## Implementation Requirements

- [ ] <Required behavior, code change, documentation update, migration, UI flow, or artifact.>
- [ ] <Required behavior, code change, documentation update, migration, UI flow, or artifact.>
- [ ] Implementation matches the End State Model diagrams, or the plan is updated before intentional divergence.

## Final Verification Standards

The goal is not done until all applicable verification items pass.

- [ ] `<command, URL, browser flow, or inspection>` passes with <expected result>.
- [ ] `<command, URL, browser flow, or inspection>` passes with <expected result>.
- [ ] End State Model review confirms the implemented workflow, architecture, data flow, infrastructure, or other diagrammed result matches the diagrams.
- [ ] If a verification item cannot be run, the final report explains why, what risk remains, and the exact command or manual check a human should run.

## Parallel Compatibility Review

Other plans reviewed:

- [ ] `./goals/<other-goal>/GOAL_PLAN.md` and diagrams - <compatible | minor coordination needed | conflict>

Parallel safety summary:

- <State whether this plan can run in parallel with the reviewed plans.>

Potential conflicts:

- <Overlapping files, migrations, config, deployments, tests, generated artifacts, or assumptions.>

Coordination notes:

- <Sequencing, split, lock, ownership, or "none".>

## Progress Log

- <YYYY-MM-DD HH:MM> - Plan created. Implementation not started.

## Final Report Requirements

When the goal run stops, report:

- Completed implementation requirements.
- Files changed.
- Final verification commands and results.
- Remaining unchecked items.
- Blockers, risks, or follow-up recommendations.
````

## Quality Bar

The goal folder is not ready for `/goal` until:

- The objective is singular and durable.
- The definition of done is testable by commands, artifacts, or observable behavior.
- The End State Model includes the diagrams needed to understand what "done" looks like, or explicitly says no diagram would add clarity.
- The constraints include safety, secrets, branch/worktree, and deployment boundaries relevant to the repo.
- Required context points to concrete files, docs, URLs, issues, or logs.
- Final verification standards are specific enough to run without inventing commands.
- Other goal folders in `./goals/` have been reviewed for parallel-run conflicts.
- Stop conditions are explicit enough to prevent Codex from guessing through high-risk ambiguity.
- The recommended `/goal` command appears in the plan and final response.

## Prompting Pattern

When using this skill interactively, steer the conversation like this:

- "I will first produce a goal-ready folder, then stop."
- "I found this answer in the repo, so I am recording it rather than asking you."
- "One decision matters before the plan is safe: <question>. Recommended answer: <answer>."
- "The goal plan is ready; use this command: `<goal command>`."
