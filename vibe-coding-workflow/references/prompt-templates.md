# Prompt Templates

Use these templates selectively. Adapt names, commands, and file paths to the repository.

## Requirements Discussion

```text
Goal: Help me clarify the requirements before implementation.

Current idea:
<user idea>

Project context:
<repo/tooling/current files>

Ask only the questions needed to remove blocking ambiguity. Focus on:
- target user and use case
- required inputs and outputs
- scope and non-goals
- success criteria
- constraints and dependencies
- testing or demo expectations

After the questions are answered, produce doc/proposal.md.
```

## Proposal

```markdown
# Proposal

## Goal

## Non-Goals

## Users And Runtime Context

## Functional Requirements

## Inputs And Outputs

## Dependencies And Constraints

## Success Criteria

## Acceptance Checks

## Assumptions

## Open Questions
```

## High-Level Design

```text
Goal: Create doc/high-level-design.md from doc/proposal.md.

Inputs:
- doc/proposal.md
- existing repository structure

Output:
- doc/high-level-design.md

Include:
- architecture overview
- module boundaries
- data flow
- external dependencies
- major design decisions
- alternatives considered
- risks and unknowns
- high-level test strategy

If any important requirement is unclear, list the question instead of inventing an answer.
```

## Detailed Design

```text
Goal: Create doc/detailed-design.md from the proposal and high-level design.

Inputs:
- doc/proposal.md
- doc/high-level-design.md
- relevant source files

Output:
- doc/detailed-design.md

Include:
- proposed file structure
- module responsibilities
- public interfaces and data contracts
- algorithms or control flow
- error handling
- configuration and environment assumptions
- migration or compatibility concerns
- test plan per module
- implementation risks
- open questions
```

## Task Split

```text
Goal: Split the detailed design into small implementation tasks.

Inputs:
- doc/proposal.md
- doc/high-level-design.md
- doc/detailed-design.md

Outputs:
- doc/tasks/<module-or-feature>.md, one per task
- doc/tasks/progress.md

Rules:
- Each task should be independently implementable and reviewable.
- Each task must name expected files and verification commands.
- Use checklists for completion criteria.
- Put cross-task state only in progress.md.
- Mark blocked tasks clearly with the missing decision or dependency.
```

## Task File

````markdown
# Task: <name>

## Objective

## Input Docs

## Expected Files

## Dependencies

## Implementation Steps

- [ ] Step 1
- [ ] Step 2

## Tests And Checks

```bash
<command>
```

## Definition Of Done

- [ ] Code implemented
- [ ] Tests added or updated
- [ ] Checks pass
- [ ] Relevant docs or state updated
````

## Progress File

```markdown
# Progress

## Current Status

## Tasks

- [ ] <task name> - <status or blocker>

## Commands Run

## Decisions

## Blockers

## Next Steps
```

## Main Agent Prompt

```text
You are the main engineering agent for this project.

Context files:
- doc/proposal.md
- doc/high-level-design.md
- doc/detailed-design.md
- doc/tasks/progress.md
- doc/tasks/*.md

Responsibilities:
1. Select the next unblocked task from progress.md.
2. Keep implementation scoped to that task.
3. Delegate only when task boundaries are clear.
4. Review diffs after each task.
5. Run the checks listed in the task file.
6. Update progress.md with status, commands, failures, and next steps.
7. Ask the user before changing product scope, destructive behavior, public APIs, or external dependencies.

Do not skip tests. Do not perform unrelated refactors. Do not overwrite user changes.
```

## Sub-Agent Prompt

```text
You are a sub-agent implementing one task.

Task file:
doc/tasks/<task>.md

Read only the context needed for this task:
- doc/proposal.md
- doc/high-level-design.md
- doc/detailed-design.md
- doc/tasks/progress.md
- the assigned task file
- relevant source files

Implement the assigned task only.

Before finishing:
- add or update focused tests
- run the task's checks
- inspect your diff
- update the assigned task status or report exactly why it is blocked

If a requirement is unclear, stop and ask instead of guessing.
```

## Verification Checklist

```markdown
## Verification

- [ ] Relevant tests pass
- [ ] Lint passes, if available
- [ ] Type checks pass, if available
- [ ] Smoke test or demo run completed, if applicable
- [ ] git diff reviewed
- [ ] No unrelated files changed
- [ ] No secrets or local-only paths introduced
- [ ] progress.md or project state updated
```
