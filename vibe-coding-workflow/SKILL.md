---
name: vibe-coding-workflow
description: Run a document-driven Vibe Coding workflow for complex AI-assisted software projects. Use when the user wants to build or refactor a non-trivial project with Codex, Claude Code, Cursor, or another coding agent; asks for vibe coding; wants requirements, design docs, task decomposition, multi-agent implementation, progress tracking, or verification loops; or needs to turn a rough product idea into an executable engineering plan.
---

# Vibe Coding Workflow

Use this skill to turn a vague or complex coding goal into a controlled engineering workflow:
requirements, design, task split, implementation, and verification. Prefer this workflow when a
single prompt would create too much ambiguity, context pressure, or unreviewable code.

## Core Rules

- Do not start implementation until the goal, scope, and success criteria are clear enough to test.
- Keep project knowledge in files, not only in chat. Use `doc/` for planning artifacts and update existing project state files when present.
- Make every phase produce an artifact that the next phase consumes.
- Keep tasks small enough for one agent pass. A task should have explicit files, steps, and checks.
- Treat AI output as a draft until verified with diffs, tests, lint, type checks, and runtime checks.
- Pause and ask the user when a requirement, product decision, data contract, destructive action, or external dependency is unclear.

## Workflow

### 1. Stabilize The Project

Inspect the repository before planning. Identify the language, framework, package manager, test
commands, lint commands, and existing conventions. If the project is new, choose conservative
defaults and prefer tooling the user already knows.

For Python projects, prefer `uv` when no project-specific tool is established. Use it for virtual
environment, dependency, and command management.

Create planning directories only when needed:

```text
doc/
  proposal.md
  high-level-design.md
  detailed-design.md
  prompt.md
  tasks/
    progress.md
```

### 2. Clarify Requirements

Interview the user or the existing codebase before writing code. Convert rough intent into
`doc/proposal.md`.

Include:

- Goal and non-goals
- Target users or runtime context
- Inputs, outputs, and external dependencies
- Functional requirements
- Constraints and assumptions
- Success criteria and acceptance checks
- Open questions

If important answers are missing, ask targeted questions before moving to design.

### 3. Design Before Implementation

Generate or update:

- `doc/high-level-design.md` for architecture, modules, data flow, storage, dependencies, and major tradeoffs.
- `doc/detailed-design.md` for module responsibilities, interfaces, file layout, algorithms, errors, tests, and edge cases.

Require the design to call out risk, ambiguity, and alternatives. Do not hide uncertainty in confident prose.

### 4. Split Tasks

Break `doc/detailed-design.md` into independent task files:

```text
doc/tasks/<module-or-feature>.md
doc/tasks/progress.md
```

Each task file must include:

- Objective
- Input docs
- Files expected to change
- Implementation steps
- Tests and checks
- Definition of done
- Dependencies on other tasks

`progress.md` must be the shared source of truth. Use checklists for task state and keep notes short.

### 5. Generate The Control Prompt

When the project is ready for agentic implementation, generate `doc/prompt.md`. It should define:

- Main agent role: coordinate, track progress, review diffs, run checks, update `progress.md`.
- Sub-agent role: implement exactly one task or module, add tests, report blockers, update task status.
- Required context files: proposal, designs, task files, progress.
- Guardrails: ask on ambiguity, avoid unrelated refactors, do not skip tests, do not overwrite user work.
- Verification commands and final reporting format.

Load detailed templates from `references/prompt-templates.md` when drafting prompts or planning docs.

### 6. Implement In Small Passes

Work from `doc/tasks/progress.md`, one task or tightly related group at a time. Before edits, inspect
the relevant files and confirm the task still matches the codebase.

For each implementation pass:

1. Select the next unblocked task.
2. Read the task file and dependent design sections.
3. Implement only the scoped change.
4. Add or update focused tests.
5. Run the task's checks.
6. Inspect `git diff`.
7. Update `progress.md` with status, commands run, failures, and next steps.

Use sub-agents only when the task boundaries are clean enough that their work can be reviewed
independently.

### 7. Verify And Close

Before calling the work done:

- Run the relevant test suite.
- Run lint and type checks when available.
- Run a smoke test or dev server for user-facing behavior when applicable.
- Inspect the final diff for unrelated edits, generated clutter, secrets, and accidental rewrites.
- Update project state or session logs when the session produced durable context.
- Report what changed, what was verified, and remaining risks.

## Templates

Use `references/prompt-templates.md` for reusable prompts and document skeletons:

- Requirements discussion prompt
- Proposal template
- High-level design template
- Detailed design template
- Task file template
- Progress file template
- Main-agent prompt
- Sub-agent prompt
- Verification checklist
