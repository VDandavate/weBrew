---
name: test-and-verify
description: Plan and run risk-based checks, then report exact commands, exact results, and what remains unproven. Use when a change, a spec unit, or a repository needs verification before it is called done. Do not use when the user wants a defect fixed, when a full quality audit is the job, or when the only available test would destroy data.
license: MIT
---

# Test and Verify

Prove what works, name what does not, and never let an untested claim pass as tested.

## When to use

- A change, a spec unit, or a whole repository needs verification.
- Someone asks whether the work is done, safe, or ready to merge.
- A release, handoff, or archival decision needs evidence.

## When not to use

- The user wants a defect fixed. Diagnose the Bug finds the cause; Build the Specs applies the fix.
- The job is a deep quality read of the code. Use Audit and Refine.
- The only available test would destroy data, hit production, or use real personal data without approval. Stop and ask.
- A single command answers the question. Run it and report the result.

## Permissions

Read-only on production code. May add or adjust tests only after explicit approval. Never weakens an assertion, skips a check, marks a test as expected to fail, disables a hook, or edits a gate to get a green result.

## Inputs

| Input | Why it is required |
|---|---|
| What is under test | A change, a spec unit, a path, or the repository |
| The project gate command | What the project itself considers passing. Use a scoped unit gate for an intermediate unit, and the full integrated gate for a completed feature |
| The stated load | One user or many, so scale checks mean something |
| Access to a safe environment | A check that cannot run safely is not a check |

If the gate command is unknown, find it before writing any test. If it does not exist, say so and record that gap.

A unit gate is the fast, discriminating subset for a unit's changed paths: static analysis, style and naming checks, and scoped tests. An integrated gate is the repository's full suite: end-to-end, system integration, and preflight checks. Where the project defines only one gate command, that command serves both roles.

## Build a risk matrix first

Test by risk, not by file. For each row, decide whether a cheap check exists and whether it can run here.

| Risk | Ask | Cheap check |
|---|---|---|
| Wrong result | Is there an input that produces a wrong answer? | One case per boundary: empty, zero, one, many, huge |
| Boundary | What happens at the first, last, and off-by-one item? | The edge inputs, not the middle |
| Failure path | Does an error get swallowed, or a write land half done? | Force the failure, inspect the aftermath |
| Trust boundary | Is input validated where it enters, and authorization checked where it acts? | One rejected case per boundary |
| Data | Can a migration, delete, or retry lose or duplicate data? | Rehearse on a copy |
| Scale | Does it hold at the stated load: races, shared state, a query per item, quadratic work? | The cheapest load proxy available |
| Time and locale | Do dates, time zones, or rounding change the answer? | One non-UTC and one rounding case |
| Integration | Does it depend on a clock, a network call, or another service? | Stub it and test both outcomes |

Order the rows by damage if wrong, not by ease of testing.

## Process

1. Read the project rules and the existing tests. Reuse the harness that is there; do not introduce a second one.
2. Fill the risk matrix for the subject. State the rows you will not test and why.
3. Run the project gate first. A gate that already fails is a finding, not a baseline.
4. Run the cheapest discriminating checks before the expensive ones.
5. Record every command exactly as run, with its exact result line and exit code.
6. Add negative and boundary cases where the matrix shows a gap.
7. Retry anything that fails once, to separate a real failure from flakiness. Never retry until green and then report only the green run.
8. Classify every result before writing the report.
9. Stop and report if a check would be destructive, or if the subject cannot be exercised at all.

## Verifying someone else's work

When another author made the change:

- Read the actual diff. Never work from a summary, a changelog, or a commit message.
- Check each claim against the artifact. A claim you did not reproduce with a command stays unverified.
- Verify as an agent that did not author the change. If that is impossible, put "verified by author" in the first line of the report.
- Reject a verification that names no command or quotes no result line.

## Classification

| Label | Meaning |
|---|---|
| Confirmed defect | A command was run and it produced a wrong result. Quote it. |
| Suspected risk | Reading the code suggests a problem that was not exercised. Say what would confirm it. |
| Coverage gap | Behavior nothing checks. Not a defect yet. |
| Unverified | A check exists but could not run here. Say why. |
| Not run | Deliberately skipped. Say why. |

Never upgrade suspected to confirmed without running it. Never let "the tests pass" stand for behavior nothing tests.

## Refusals

Refuse, and offer a safe alternative, when a check would:

- Write to production, or to a shared environment others depend on.
- Delete or migrate real data, or read real personal data.
- Cost money, send messages, or call a third-party service with side effects.
- Take so long that it blocks the work. Narrow the check instead.

## Evidence format

```text
Subject:     <what was tested>
Load assumed: <one user | many processes | unknown>
Gate:        <exact command> / <exact result line> / exit <code>

Check:       <what risk it covers>
Command:     <exact command>
Result:      <exact result line>
Verdict:     passed | failed | flaky | not run

Confirmed defects
- <location> / <command> / <observed result> / <expected>

Suspected risks
- <location> / <why> / <what would confirm it>

Coverage gaps
- <behavior nothing checks>

Unverified
- <check> / <why it could not run>

Not run
- <check> / <why it was skipped>
```

## Output

One verification report: the risk matrix, the exact commands and results, the classification, and the residual risk. Tests only if they were approved and added.

## Done when

Every risk row is tested, refused, or explicitly unverified. Every claim names the command that produced it. A reader who was not present can tell what is proven, what is suspected, and what is unknown.
