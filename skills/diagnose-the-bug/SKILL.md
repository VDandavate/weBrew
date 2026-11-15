---
name: diagnose-the-bug
description: Find the root cause of a defect or regression with a tight reproduction loop, confirm it with evidence, and leave a failing regression test. Use when something is broken, slow, or regressed and the cause is unknown. Do not use when the cause is already known, when the fix is the job, or when the task is feature work.
license: MIT
---

# Diagnose the Bug

Find the cause. Leave the fix to someone who can be reviewed.

## When to use

- Something fails, crashes, hangs, or returns a wrong result, and the cause is unknown.
- A regression appeared and the change that caused it is unclear.
- A report arrives that nobody can reproduce yet.

## When not to use

- The cause is known and the fix is the job. Write the spec unit and build it.
- The task is new behavior. That is feature work.
- The user wants a workaround shipped now. State that the cause stays unknown and record the debt explicitly.

## Permissions

Read-only by default. May add a failing regression test with approval. Does not apply the fix, does not change production code, and does not silence the symptom.

## Inputs

| Input | Why it is required |
|---|---|
| The symptom, in the reporter's words | What to reproduce, before anyone rewords it |
| Expected and actual behavior | What "wrong" means here |
| The environment | Version, platform, configuration, data shape |
| How to run the thing | The command or entry point that exercises it |
| The last known good state | A commit, a release, or a date to compare against |

Missing inputs are part of the diagnosis. Ask for the one that blocks reproduction, not for all of them.

## Process

### 1. State the symptom precisely

One sentence: this input or action, in this environment, produces this result instead of that one. Vague symptoms produce vague causes.

### 2. Build a failing signal

Find the cheapest command that fails for the right reason, every time. A test, a script, a request, a one-line reproduction.

- If it will not fail, you do not have the bug yet. Narrow the conditions.
- If it fails intermittently, record the rate and the conditions before continuing.
- If it cannot be reproduced at all, say so, list what you tried, and ask for the missing input. Never guess a cause from a symptom.

### 3. Minimize

Strip the reproduction until removing anything else makes the failure disappear. Fewer inputs, fewer steps, less data. The minimized case is usually the explanation.

### 4. Locate

- Follow the code path from the observed failure back to where the wrong value is created.
- Bisect: use version control across commits, or disable configuration, data, or dependencies one at a time.
- Add temporary instrumentation to see the actual values. Mark every temporary line and remove all of them before reporting.
- Compare with the last known good state and name the difference.

### 5. Test hypotheses

Write two or more candidate causes before testing any. For each, state what evidence would separate it from the others, then run the cheapest discriminating check first.

A cause is confirmed only when both hold:

1. It predicts the observed symptom, including the details that ruled out the alternatives.
2. Changing only that cause changes the symptom, and changing it back brings the symptom back.

### 6. Write the regression test

The test targets the cause, not the symptom. It fails now, for the reason diagnosed, and it will pass once the cause is fixed. Add it only with approval.

### 7. Hand off

Report the cause, the mechanism in one paragraph, the minimized reproduction, the discriminating evidence, and the regression test. Name the fix as a scope for a spec unit. Do not fix it here.

## Stop conditions

| Situation | Response |
|---|---|
| Cannot reproduce | Report conditions tried, request the missing input, do not guess |
| Intermittent or timing dependent | Record the rate and the conditions; treat a race as a hypothesis, not a cause |
| Only reproduces in production | Ask for the smallest safe observation: a log, a metrics slice, a redacted sample |
| Cause is a design decision | Report it as intended behavior and name who can change the decision |
| Several independent causes | Diagnose each separately, with its own evidence, and say so |
| The bug needs real personal data | Stop. Ask for a redacted or synthetic reproduction |

## Evidence

```text
Symptom:      <one sentence: input, environment, actual, expected>
Reproduction: <exact command or steps> / <failure line>
Minimized:    <the smallest failing case>
Last good:    <commit, release, or date>

Cause:        <file:line and the one-sentence mechanism>
Why:          <the evidence that discriminates this cause from the alternatives>
Toggled:      <what was changed to prove the cause, and what happened>
Regression test: <path> / <fails now with this line> / <approved: yes or no>
Fix scope:    <the change a spec unit should make>
Not proven:   <what this diagnosis does not explain>
```

## Output

A reproduced defect, a confirmed cause with evidence, a red regression test if approved, and a fix scope. No production change.

## Done when

The cause explains the symptom, the evidence separates it from the alternatives, the reproduction is minimal and repeatable, and someone else can now write the fix without rediscovering anything.
