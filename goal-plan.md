---
name: goal-plan
description: Create or update a self-contained goal-ready plan artifact for Codex `/goal` runs. Use when the user wants a plan, charter, checklist, definition of done, constraints, validation loop, stop conditions, or handoff artifact that lets them later run a terse goal such as `/goal Implement GOAL_PLAN.md.`
---

# Prepare Goal Plan

## Overview

Produce a Markdown artifact that is strong enough for a long-running Codex goal: clear objective, bounded scope, repo-grounded context, validation loop, checkpoint plan, and explicit stop conditions.

This skill prepares the work. Do not implement the plan unless the user explicitly asks.

## Workflow

1. Establish the goal shape.
   - Interview me relentlessly about every aspect of this plan until we reach a shared understanding. Walk down each branch of the design tree, resolving dependencies between decisions one-by-one. For each question, provide your recommended answer.
   - If the answer can be found by reading the repo, docs, issues, logs, or config, inspect those instead of asking.
   - Record assumptions in the artifact when moving forward without a user answer.

2. Ground the plan.
   - Read the relevant `AGENTS.md`, README, architecture docs, existing plans, tests, source files, and deployment notes.
   - Check the working tree before editing.
   - Preserve unrelated user changes.
   - Note unresolved unknowns instead of pretending they are known.

3. Create or update one durable plan artifact.
   - Prefer an obvious repo-local file that a later goal can find quickly.
   - Use the path structure `./goals/GOAL_PLAN_<custom name of goal>.md`.
   - Keep the artifact self-contained enough that a future Codex run can execute it after reading only the plan plus listed context files.

4. Finish with a goal handoff.
   - Include a "Goal Handoff" section in the artifact with the recommended `/goal` command.
   - Make the command terse, but not brittle. Prefer `/goal Implement the plan in <path>.`
   - In the final response, link the artifact and paste the recommended `/goal` command.

## Artifact Template

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

## Objective

<One durable outcome Codex should achieve.>

## Definition Of Done

- [ ] <Observable final condition.>
- [ ] <Required tests, builds, browser checks, deployments, or artifacts pass.>
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

## Assumptions To Preserve Or Validate

- [ ] <Assumption and how to validate it, if validation is possible.>

## Stop Or Pause Conditions

- [ ] Stop and ask before <destructive action, production action, secret exposure, large scope change, unresolved product decision, or human approval>.
- [ ] Pause if <verification failure or ambiguity> means the plan itself needs revision.

## Checkpoints

### Checkpoint 1: <Name>

Purpose: <Why this checkpoint exists.>

Tasks:

- [ ] <Task>
- [ ] <Task>

Acceptance criteria:

- [ ] <Observable pass condition>

Verification:

- [ ] `<command or manual check>`

### Checkpoint 2: <Name>

...

## Validation Loop

After each checkpoint:

- [ ] Run the checkpoint verification.
- [ ] Update completed checkboxes.
- [ ] Add a progress log entry.
- [ ] Re-read stop conditions before continuing.

## Progress Log

- <YYYY-MM-DD HH:MM> - Plan created. Implementation not started.

## Final Report Requirements

When the goal run stops, report:

- Completed checkpoints.
- Files changed.
- Verification commands and results.
- Remaining unchecked items.
- Blockers, risks, or follow-up recommendations.
````

## Quality Bar

The artifact is not ready for `/goal` until:

- The objective is singular and durable.
- The definition of done is testable by commands, artifacts, or observable behavior.
- The constraints include safety, secrets, branch/worktree, and deployment boundaries relevant to the repo.
- Required context points to concrete files, docs, URLs, issues, or logs.
- Every checkpoint has verification.
- Stop conditions are explicit enough to prevent Codex from guessing through high-risk ambiguity.
- The recommended `/goal` command appears in the artifact and final response.

## Prompting Pattern

When using this skill interactively, steer the conversation like this:

- "I will first produce a goal-ready artifact, then stop."
- "I found this answer in the repo, so I am recording it rather than asking you."
- "One decision matters before the plan is safe: <question>. Recommended answer: <answer>."
- "The artifact is ready; use this command: `<goal command>`."
