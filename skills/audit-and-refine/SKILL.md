---
name: audit-and-refine
description: Audit a repository, a path, or a commit range; explain what it does; and rank what to fix and what to delete. Then apply approved, behavior-preserving cleanups. Use when code needs a deep quality read, or when approved dead code, duplication, or structure should be removed. Do not use when behavior must change, or when you only want a diff reviewed.
license: MIT
---

# Audit and Refine

Understand first, judge second, delete third, and only with permission.

## When to use

- A repository, a module, or an area needs a serious quality read.
- You inherited code and need to know what it does before changing it.
- Approved dead code, duplication, or structure should be removed.
- Someone asks what is wrong, what is unused, or what is safe to delete.

## When not to use

- Behavior must change. Bugs and features belong to a spec and to Build the Specs.
- You only want one commit reviewed. Read the diff directly; this skill reads whole areas.
- You want a test plan executed. Use Test and Verify.
- Nobody has approved any cleanup yet. The audit phase still applies; the refine phase does not.

## Permissions

Two phases with different permissions.

**Audit: read-only.** Reads the repository, runs safe checks, writes a report. Never edits a file.

**Refine: writes only after explicit approval.** Changes only the approved items, keeps behavior identical, and commits one cleanup at a time. Never pushes, merges, rewrites history, tags, or bypasses hooks.

Never move from audit to refine without an explicit yes naming the items.

## Phase 1: Audit

### 1. Set the scope

Name what you are auditing and how deep: the whole repository, a path, or a commit range. Read the project's own rules and standards first. If the project has no written standards, say so and skip that axis rather than importing your own taste.

State the scope and what you will not read before you start.

### 2. Map it

- Read the README, the build and deploy config, the dependency list, the entry points, and the tests.
- Establish the expected load: one person running a script, or many processes at once. Judge scale against that, and say which load you assumed.
- Trace the main flows end to end: where data enters, what is stored, what leaves.
- Big area: go deep where a mistake costs the most, not file by file.

### 3. Understand it

Write a short model of the code before judging it:

- What it does, in two or three sentences.
- The handful of modules that matter and the job each one does.
- The invariants that must hold, in plain words.
- Where state lives and who owns it.

If you cannot write that model, you have not understood the code yet. Say what is unclear instead of guessing.

### 4. Verify the claims

Run the gates and any documented command that is safe. Check what the docs claim against what the code does. Record exact commands and results.

### 5. Find defects

- **Bugs.** Empty, zero, last item, rounding, time zones. Callers that disagree with what a function returns. The same rule applied two ways.
- **Security.** Injection, weak randomness, secrets in code, missing checks on input that crosses a trust boundary, missing authorization.
- **Data loss.** Errors swallowed, writes in the wrong order, no transaction, a delete that cannot be undone.
- **Scale.** Check-then-write races, per-process state that must be shared, memory or lists that only grow, a query per item, quadratic work on large input.

### 6. Find debt

- **Delete.** Dead code, unused options, flags, config, and dependencies, speculative features.
- **Reuse.** Two helpers doing the same job. Keep one and name the path.
- **Standard library.** A dependency doing what the platform or a few lines already do.
- **Speculation.** An interface with one implementation, a factory with one product, a wrapper that only passes calls through.
- **Merge.** Near-copies that must change together.
- **Split.** One function or class doing several unrelated jobs, so it is hard to read or test. Split by job, never by line count.
- **Standards.** Code that breaks the project's own documented rules.

### 7. Rank and prove

Sort findings into three groups: must fix, should fix, nice to have.

Every finding needs a concrete case: this input or situation, this wrong result. No case, no finding.

Before calling code unused, search the whole tree for it, including tests, config, and string or dynamic references. Record the search you ran.

A shortcut comment that names its limit is a decision, not a finding, unless the assumed load already crosses it.

### 8. Plan the cleanup

Separate the report into two lists:

- **Fix.** Changes behavior. These become spec units under Build the Specs. This skill does not do them.
- **Clean.** Removes or reshapes code without changing behavior. These are candidates for Phase 2.

For each clean item: what proves it is unused or redundant, what would break if it goes, and the order to remove it in.

## Phase 2: Refine

Only after the user approves named items from the clean list.

1. Restate the approved items and the scope. Nothing else may change.
2. Characterize current behavior for each item: a check, a test, or a recorded before output.
3. Apply the smallest reversible change, one item at a time.
4. Run the gate after each item. Compare behavior to the characterization.
5. Commit that one cleanup, named for what it removed.
6. Stop if behavior changes, if the change needs files outside the item, or if the gate was already failing.

One approved item per commit. Never bundle a cleanup with a bug fix, a feature, or a reformat.

## Rules

- Never change observable behavior during refine.
- Never fix a bug in refine. It is a fix, not a cleanup, and it belongs in a spec.
- Never reformat a file wholesale to make a cleanup easier.
- Deletion beats addition. If two options are the same size, take the one that removes code.
- Never weaken an assertion or a gate to make a cleanup pass.
- No style taste, no "consider", no vague worries.
- Stage explicit paths only. Never use `git add -A` or `git add .`.

## Finding format

```text
1. **Orders land on the wrong day** (billing/closeDay.py:L40-52)
   What this is: At midnight this job bills every order placed that day.
   Problem: It reads "today" from the server clock in UTC, so an order placed at 00:30 Berlin time bills to the day before.
   Fix: Compute the day once in the shop time zone. One line, nothing else changes.
   If we skip it: Late orders carry the wrong date and someone fixes them by hand.
```

## Report format

```text
What this does: <two or three sentences>
Load assumed: <one user | many processes | unknown>

Must fix
1. ...

Should fix
7. ...

Nice to have
12. ...

Clean list
A. <what to remove>, proof: <search or reference>, risk: <what breaks>
B. ...

Verdict: <healthy | fix 1, 3, 7 first>
Lean: -<N> lines, -<M> dependencies possible
Not checked: <what you did not read or could not run>
```

Number findings across groups so the user can say "fix 2 and 5". At most twenty findings; if you left smaller ones out, say how many.

## Output

**Audit:** the model, the report, the ranked findings, and the two lists.
**Refine:** one commit per approved item, behavior proven unchanged, and the line and dependency delta.

## Done when

**Audit:** the reader can say what the code does, what to fix first, and what to delete. Every finding has a case and a location. Nothing was edited.
**Refine:** every approved item is applied or reported as blocked, behavior is proven unchanged, and nothing outside the approved list changed.
