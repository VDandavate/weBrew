---
name: assure-quality
description: "Coordinate a quality pass across specialists: assess, propose, wait for approval, dispatch audit, refinement, and verification, then require independent verification before calling anything done. Use when a bounded remediation or a whole-repository quality pass needs more than one skill. Do not use when one check settles the question, or when remediation has not been approved."
license: MIT
---

# Assure Quality

Run the quality pass. Do not grade your own homework.

## When to use

- A bounded remediation or a whole-repository quality pass needs several skills.
- Work is claimed as done, and nobody independent has checked it.
- Several findings need ordering, ownership, and a single reconciled answer.

## When not to use

- One check settles the question. Run it.
- Remediation is not approved yet. Nothing gets dispatched.
- There is no change or repository to assess.
- The user wants a fix in one area. Diagnose the Bug and Build the Specs handle that directly.

## Permissions

Coordinates. Does not reimplement specialist work. Does not write code. Never approves its own output, and never accepts a specialist's own claim as verification.

Read-only until a batch is approved. May stage explicit paths and commit approved batches when the user authorizes it.

## Roles

| Role | Owns | May not |
|---|---|---|
| Lead | Scope, order, dispatch, reconciliation | Judge its own dispatch, edit specialist output |
| Auditor | Findings and the clean list | Apply a change |
| Implementer | Approved changes only | Widen scope, approve itself |
| Verifier | Independent evidence | Be the author of what it verifies |
| User | Approval, priority, delivery | — |

At least two distinct agents are required for a real pass. Assign the roles before the first batch moves:

1. Several subagents available: one subagent per role. The Lead dispatches the Auditor. The Implementer receives only approved batches. The Verifier receives the diff and the gate command, never the Implementer's summary or reasoning.
2. One agent, sequential turns: run the roles as persona-isolated turns. Close the previous turn's context, state the role you are taking and what that role may not do, and treat each role's output as a written artifact the next role reads cold.
3. No way to separate the roles: say so, label the result "verified by author" rather than independent, and let the user decide.

## Process

### 1. Bound the pass

State the question being answered, the scope, the exit criteria, and the effort budget. A quality pass that cannot say what would end it will not end.

### 2. Assess or dispatch the assessor

Collect the evidence that already exists: gate results, status reports, known defects, open questions. If the picture is unclear, dispatch Audit and Refine in its read-only phase.

### 3. Propose batches

Group the work into batches, each with one owner and one purpose.

| Batch | Purpose | Expected effect | Risk | Depends on |
|---|---|---|---|---|
| 1 | Behavior-preserving cleanup of dead code | Fewer lines, same behavior | Low | — |
| 2 | Fix the wrong-day billing defect | Correct dates | Medium | Spec unit approved |
| 3 | Independent verification of 1 and 2 | Evidence, not claims | — | 1, 2 complete |

Rules for batches:

- Behavior change and behavior preservation never share a batch.
- Independent batches may run in parallel only with exclusive, non-overlapping files.
- Dependent batches run in order.
- One writer per file, always.

### 4. Wait for approval

Present the batches and stop. Approval must name the batches. Silence is not approval, and a general "go ahead" on the assessment is not approval to change code.

### 5. Dispatch

Send each approved batch to the skill that owns it, with its scope, acceptance, and the gate command. Never dispatch a batch whose dependency is incomplete. Never let the implementer also verify.

### 6. Verify independently

A different agent re-runs the gates, reads the actual diff, and checks each claim against the artifact. A summary from the implementer is a claim, not evidence.

Reject a verification that names no command or quotes no result line.

### 7. Reconcile

Merge the reports into one picture:

- What is now proven, and by which command.
- What is still suspected, and what would confirm it.
- What was not touched.
- What the pass left open.

Unresolved differences are reported, never smoothed over by the lead.

### 8. Report

One report, plainly written, with the batches, the evidence, and the remaining risk.

## Rules

- Never self-approve, and never let one agent author, approve, and verify the same batch.
- Never weaken an assertion, skip a check, or edit a gate to close a batch.
- Never grow the scope between approval and dispatch. A new finding becomes a new proposal.
- Never let two batches write the same file.
- Never present a specialist's summary as verification.
- Report exact commands and exact result lines.
- Stop and ask when ownership, approval, or the destination of a change is unclear.

## Evidence

```text
Pass:        <the question this pass answered>
Scope:       <what was assessed, what was excluded>
Batches:     <n> approved, <n> complete, <n> blocked

Batch <id>:  <purpose>
Owner:       <agent or skill>
Gate:        <exact command> / <exact result line>
Verified by: <a different agent>
Result:      accepted | rejected / <the specific gap>

Reconciled
Proven:      <claims with their commands>
Suspected:   <risk and what would confirm it>
Untouched:   <what the pass did not cover>
Open:        <decisions the user must make>
Independence: <independent | verified by author, limited>
```

## Output

An approved, ordered, independently verified quality pass with one reconciled report. No unreviewed change is described as done.

## Done when

Every approved batch is complete or blocked with a named reason, every claim is backed by a command a different agent ran, and the remaining risk is stated plainly enough for the user to decide what happens next.
