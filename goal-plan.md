---
name: goal-plan
description: Create or update a self-contained goal-ready plan artifact for Codex `/goal` runs. Use when the user wants a plan, charter, checklist, definition of done, constraints, final verification standards, stop conditions, or handoff artifact in `./goals/` that lets them later run a terse goal such as `/goal Implement the plan in ./goals/GOAL_PLAN_example.md.`
---

# Goal Plan

## Overview

Produce a Markdown artifact that is strong enough for a long-running Codex goal: clear objective, bounded scope, repo-grounded context, explicit stop conditions, final verification standards, and a compatibility check against other goal plans.

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

4. Define final verification standards.
   - Do not use checkpoints as the main execution structure. Let the `/goal` feature manage its own working cadence.
   - Include final verification that must pass before the goal can be called done.
   - Prefer exact commands, URLs, browser flows, test files, lint/build steps, deployment checks, and artifact inspections.
   - For every verification item, state the expected pass condition and what to report if it fails.

5. Check parallel-plan compatibility.
   - Before marking the plan ready, inspect other Markdown plans under `./goals/`.
   - Compare the new plan with existing goal plans for likely conflicts if run in parallel.
   - Look for overlapping files/directories, schema or migration changes, dependency/lockfile changes, shared config/env changes, test fixture changes, Docker/deployment changes, branch/git operations, generated artifacts, and competing assumptions.
   - Add a "Parallel Compatibility Review" section to the artifact.
   - If conflicts are minor, list coordination notes. If conflicts are massive or unsafe, leave status as `Draft` or `Blocked` and explain what must be sequenced or split before running goals in parallel.

6. Finish with a goal handoff.
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

## Assumptions To Preserve Or Validate

- [ ] <Assumption and how to validate it, if validation is possible.>

## Stop Or Pause Conditions

- [ ] Stop and ask before <destructive action, production action, secret exposure, large scope change, unresolved product decision, or human approval>.
- [ ] Pause if <verification failure or ambiguity> means the plan itself needs revision.

## Implementation Requirements

- [ ] <Required behavior, code change, documentation update, migration, UI flow, or artifact.>
- [ ] <Required behavior, code change, documentation update, migration, UI flow, or artifact.>

## Final Verification Standards

The goal is not done until all applicable verification items pass.

- [ ] `<command, URL, browser flow, or inspection>` passes with <expected result>.
- [ ] `<command, URL, browser flow, or inspection>` passes with <expected result>.
- [ ] If a verification item cannot be run, the final report explains why, what risk remains, and the exact command or manual check a human should run.

## Parallel Compatibility Review

Other plans reviewed:

- [ ] `./goals/<other-plan>.md` - <compatible | minor coordination needed | conflict>

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

The artifact is not ready for `/goal` until:

- The objective is singular and durable.
- The definition of done is testable by commands, artifacts, or observable behavior.
- The constraints include safety, secrets, branch/worktree, and deployment boundaries relevant to the repo.
- Required context points to concrete files, docs, URLs, issues, or logs.
- Final verification standards are specific enough to run without inventing commands.
- Other plans in `./goals/` have been reviewed for parallel-run conflicts.
- Stop conditions are explicit enough to prevent Codex from guessing through high-risk ambiguity.
- The recommended `/goal` command appears in the artifact and final response.

## Prompting Pattern

When using this skill interactively, steer the conversation like this:

- "I will first produce a goal-ready artifact, then stop."
- "I found this answer in the repo, so I am recording it rather than asking you."
- "One decision matters before the plan is safe: <question>. Recommended answer: <answer>."
- "The artifact is ready; use this command: `<goal command>`."
