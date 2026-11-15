---
name: check-status
description: Read version control, specs, approvals, and ownership, reconcile what disagrees, and report what is complete, in progress, blocked, and unverified. Use when someone asks where things stand, or when a session resumes. Do not use when the user wants work done, a fix, or a decision made.
license: MIT
---

# Check Status

Say where things stand. Change nothing.

## When to use

- The user asks what is done, what is left, or what is blocked.
- A session resumes and the state must be rebuilt from artifacts, not memory.
- Before choosing the next piece of work.

## When not to use

- The user wants the next unit built. Use Build the Specs.
- The user wants a quality read. Use Audit and Refine.
- The ask is a defect diagnosis. Use Diagnose the Bug.

## Permissions

Read-only. Never commits, edits records, moves files, or updates state. If the records are wrong, report the disagreement and name the skill that owns the correction.

## Sources, in order of trust

1. Version control: branch, staged and unstaged changes, untracked files, recent commits.
2. Verification evidences: exact commands and results.
3. Spec and unit state.
4. Approval records.
5. Ownership and dispatch claims.
6. Memory, conversation, or a summary from another agent: last, and never as the only source.

Where a higher source contradicts a lower one, the higher one wins and the difference is reported.

## Process

1. Read the project rules and confirm the repository root and branch.
2. Read version control state, including uncommitted and untracked work.
3. Read every spec: units by state, dependencies, approvals.
4. Read verification evidence. A commit without a result line is unverified, not complete.
5. Build the table, one row per unit.
6. Classify each row: complete, in progress, blocked, unverified, not started.
7. List contradictions between sources, without fixing them.
8. State what can be done next, and what each next action would need.

## Classification

| Label | Evidence required |
|---|---|
| Complete | A commit and a recorded result that still holds |
| In progress | Uncommitted changes belonging to that unit |
| Blocked | A named blocker and who can clear it |
| Unverified | Committed, but no result line, or no independent check |
| Not started | No commit and no working changes |

Never round "probably done" up to complete. A unit whose check no longer passes is not complete, whatever the records say.

## Report

```text
Repository:  <root> / branch <name> / <clean | n modified, n staged, n untracked>
Rules:       <path to the project rules, or none found>
Gate:        <exact command> / <last recorded result, with its date source>

State
| Unit | Spec | State | Evidence | Blocker |
|---|---|---|---|---|
| ...  | ...  | complete | <commit, result line> | — |

Counts:      <n> complete, <n> in progress, <n> blocked, <n> unverified, <n> not started

Contradictions
- <source A says X, source B says Y, I trust A because ...>

Next
- <action> / needs: <approval, dependency, or decision>
- <action> / needs: ...

Not checked
- <what was not read, or could not be verified>
```

## Rules

- Report; never repair. Correcting a record is someone else's unit of work.
- Never infer completion from a summary, a commit message, or a plan.
- Never report a count you did not derive from the artifacts.
- Never hide an inconsistency to make the report tidy.
- Redact secrets, tokens, and personal data.
- Say "not checked" rather than implying a check passed.

## Output

One status report: artifact-derived state, honest counts, contradictions, and the next safe actions. No changes to the repository.

## Done when

A reader who just arrived can name what is complete, what is not, what blocks it, and which action is safe to take next.
