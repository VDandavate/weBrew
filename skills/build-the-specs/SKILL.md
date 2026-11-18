---
name: build-the-specs
description: Find the next unfinished spec, or build a named one, unit by unit, with a failing check, a project gate, and a commit for each unit. Use when specs exist and some work remains, or when the user asks what to build next. Do not use when no spec exists, or when the change is a single small edit.
license: MIT
---

# Build the Specs

Pick the right spec, then take it from unbuilt to built, unit by unit.

## When to use

- A project has one or more specs and some work remains.
- The user asks what to build next, what is left, to continue, or to resume.
- The user names a specific spec to build.

## When not to use

- No spec exists. Use Grill Ideas to Specs first.
- The change is one small edit with an obvious outcome. Do it directly.
- The work is a quality audit. Use Audit and Refine.
- The user wants a status report and no building. Use Check Status.

## Permissions

Writes code within the units of one spec, and unit state inside that spec. Never pushes, merges, rewrites history, tags, releases, or bypasses hooks. Never edits another spec's state.

## The spec model

One spec describes one feature. Read any spec into this shape before building.

```text
spec   one feature
└── work unit
    ├── scope        files and behavior it may change
    ├── acceptance   a statement someone else can check
    ├── depends on   other units, or another spec, by name
    └── state        not started | in progress | complete | blocked
```

If the spec does not fit this shape, say which parts are missing and stop. Do not invent units to fill gaps. A spec that bundles several unrelated features is a planning defect: report it rather than building all of them.

## Two ways in

| Invocation | What you do |
|---|---|
| No spec named | Inventory every spec, report the unfinished work, choose the next eligible one, build it, stop |
| A spec named | Build that spec, stop |

One spec per run. When it is done, report and stop. Never continue into the next spec on your own.

## Checking the specs

This happens on every run, named spec or not, because the state may be stale.

For each spec, count its units: not started, in progress, complete, blocked.

| Result | Meaning |
|---|---|
| Every unit complete | The spec is finished. It needs archival, not building. Skip it. |
| Some units complete, none in progress | Partly built. A candidate. |
| A unit in progress | Mid-run. Finish or discard that unit before anything else. |
| Any unit blocked | Report the blocker. Do not route around it. |
| No units at all | Not broken down. Send it back for planning. |

Report the counts before you start building. A build that cannot say how many units are unfinished has not read the specs.

## Choosing the spec

A spec is eligible when all of these are true:

1. It has a recorded approval.
2. It has at least one unfinished unit.
3. Every spec it depends on is complete.
4. No other active work is writing the same files.
5. Its units have scope and acceptance statements.

If several are eligible, prefer the one that unblocks the most other specs. If they are equal, ask the user rather than inventing a priority.

If the user named a spec that fails these checks, say which check it fails and stop.

## Entry points

| State | How you know | What you do |
|---|---|---|
| Fresh | Approved, no unit has a commit | Start with the first unit in dependency order |
| In progress | Some units committed, none half done | Continue with the next unit |
| Interrupted | A unit was started and left uncommitted | Inspect the tree, then finish or discard that unit first |
| Blocked | A gate failed or a dependency is missing | Report it. Do not work around it |

A fresh build is the normal case, not an edge case.

## Inputs

| Input | Why it is required |
|---|---|
| The spec | What to build, and how it is broken down |
| Approval | Evidence the spec is allowed to be built |
| Units with acceptance | How each unit is proven |
| A scope per unit | Which files each unit may change |
| The gate command | What must pass before each commit |

If any input is missing, stop and ask.

## Gate tiers

Two tiers of the project gate exist. Use the one that matches what you are about to commit.

| Tier | Covers | Run before |
|---|---|---|
| Unit gate | Fast, discriminating checks: static analysis, style and naming checkers, and tests scoped to the unit's changed paths | Every intermediate work-unit commit |
| Integrated gate | The repository's full verification suite: end-to-end, system integration, and preflight checks | The spec's final integration unit, or final handoff |

Single-gate fallback: when the project defines only one gate command, that command serves both tiers. Say so in the evidence rather than inventing a second command.

## Rules

- One commit per unit. Never one commit for the whole spec.
- Test first, for every unit. The failing check exists before the implementation.
- Build in dependency order. Never skip a unit silently; report what you skipped and why.
- Never widen a unit's scope. If a unit is wrong, that is a plan defect; stop and report it.
- Never weaken an assertion, skip a check, mark a test as expected to fail, disable a hook, or edit a gate to get green.
- No secrets, credentials, or personal data in code, output, logs, or commit messages.
- Follow the conventions already present in the files you touch.
- Update the spec's records as you go, one unit at a time. Never batch the record at the end.
- Stage explicit paths only. Never use `git add -A` or `git add .`.

## Process

### 1. Read and choose

Read the project rules, the specs, the current branch, and the working tree. Run the spec check above. Choose the spec, or use the one named. Restate its unit order and acceptance before writing anything.

### 2. Order the units

Sort units so every unit comes after the units it depends on. While sorting:

- A unit that depends on another spec's unit cannot start until that unit is complete.
- A cycle in the dependency graph is a plan defect. Stop and report it.
- A unit with no acceptance statement is not buildable. Stop and report it.

### 3. Build each unit

For every unit, in order:

1. Restate the unit's scope and acceptance in two sentences. If you cannot, stop.
2. Write the smallest failing check that expresses the missing behavior. Run it and confirm it fails for the right reason.
3. Implement the smallest change that makes it pass.
4. Run the applicable unit gate, not only the new check. On the spec's final integration unit, run the full integrated gate. Capture the exact result line.
5. Record the unit's evidence and mark its state complete in the spec and progress record.
6. Stage the explicit implementation files, test files, and updated spec records together.
7. Read your own diff once, as a reviewer would. Fix what you would flag.
8. Commit once for the unit, naming the unit outcome and nothing about process.

### 4. Stop conditions

Stop and report instead of working around any of these:

- A dependency is incomplete or missing.
- The gate was already failing before you touched the unit.
- The unit needs files outside its scope.
- The acceptance statement cannot be checked.
- The spec contradicts the project rules.
- Two units need to change the same file at the same time.
- The spec named by the user fails an eligibility check.

### 5. Hand off

When every unit is complete, report the spec as built, with the per-unit evidence and the diff range. Do not archive it, do not push, do not merge, and do not start another spec.

## After an interruption

Read the branch, the last commits, and the specs. Trust Git over the records when they disagree, and fix the records to match.

Answer three questions before building:

1. Was a unit started and never committed? Its changes must be completed or reverted.
2. Did a commit land without a gate result? That unit is unverified, not complete.
3. Do the specs claim work that Git cannot show? Fix the records first.

## Cross-spec dependencies

A unit may depend on a unit in another spec.

- The dependency is satisfied only by a completed unit with a commit, not by a plan.
- A cycle across specs is a planning defect. Report it; never break it by guessing an order.
- A spec that blocks two or more others is built first unless the user says otherwise.

## Failure modes and responses

| Symptom | Response |
|---|---|
| The failing check cannot be made to fail | The behavior already exists, or the check is wrong. Confirm which first. |
| The gate fails inside the unit | Fix the cause, rerun, record both result lines. |
| The gate fails outside the unit | Stop. Report the pre-existing failure with its result line. |
| The check passes but acceptance is unmet | The check is too weak. Strengthen it before committing. |
| A unit is too large to finish | Report the split you recommend. Never build half a unit and call it complete. |
| Two units collide in one file | Stop. Report the collision rather than choosing an order silently. |

## Evidence

One block per unit, plus a spec summary.

```text
Spec:        <spec name>
Unit:        <unit name>
Acceptance:  <the acceptance statement>

Red:         <command> / <failure line>
Green:       <command> / <result line>
Gate:        <exact command> / <exact result line>
Commit:      HEAD (staged with this unit) | <subject>
Scope:       <files changed>
Not covered: <what this unit leaves unproven>
```

The evidence block is committed with its unit.

The summary and diff range belong to the handoff report, written after the last commit:

```text
Units:       <complete>/<total> complete, <n> blocked
Diff range:  <first commit>..<last commit>
```

## Output

One built spec: a commit per unit, a passing gate per unit, unit state updated as each unit lands, and the evidence blocks. Nothing else.

## Done when

Every unit of the chosen spec is complete with its own commit and gate result, or blocked with a named reason. The spec state matches Git. The next spec was not started.
