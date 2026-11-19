---
name: archive-the-specs
description: Retire a completed spec after verifying its evidence, preview the move, get explicit approval, and archive it without overwriting an existing entry. Use when every unit is complete, verified, and the user wants the records retired. Do not use when work is unfinished, evidence is missing, or approval has not been given.
license: MIT
---

# Archive the Specs

Retire finished records. Never lose one, never overwrite one.

## When to use

- Every unit of a spec is complete, with a commit and a recorded result.
- Independent verification exists for the work that changed code.
- The user asks to archive, retire, or clean up finished records.

## When not to use

- Any unit is incomplete, blocked, or unverified.
- The evidence is missing or is only a claim.
- Another open spec depends on the one being archived.
- The user has not approved the move. Read the record, prepare the preview, then ask.

## Permissions

Moves records only after a preview and an explicit yes. Never overwrites an existing archive entry, never deletes, never rewrites history, never pushes.

Read-only until the move is approved.

## Verify before moving

Check each item. A single failure stops the archival.

| Check | Passing looks like |
|---|---|
| Every unit complete | Each unit has a commit and a recorded result |
| No unit blocked | No named blocker remains open |
| Verification present | An exact command and result for the work that changed code, from someone other than the author |
| No dependent open work | No other spec lists this one as a dependency and remains unfinished |
| Records consistent | The spec state matches version control |
| Clean tree in scope | No uncommitted changes under the records being moved |
| Work-product rules | The destination is a records location, not a published surface |

## Check the destination

- Does the archive location exist? If not, ask where it should be; do not invent one.
- Does an entry with the same name already exist? Resolve it before anything else:
  - Byte-identical content: stop and ask before replacing.
  - A successor, later phase, or redesign of an already-archived spec: never overwrite the prior entry. Propose a structured revision directory such as `<destination>/<featureName>/v2` or `<destination>/<featureName>-v2`, and record the exact path of the archived spec it succeeds.
  - Unrelated work that merely shares the base name: ask the user which it is. Do not guess.
- Is the destination inside a directory that is itself published? Archiving into a published area is a defect, not a cleanup.
- Are other records linking to the path being moved? Note every reference that will break.

## Preview

Before moving anything, show:

```text
Source:       <exact path or paths>
Destination:  <exact path>
Revision:     <spec revision or tag at handoff>
Successor to: <none | exact path to prior archived version>
Method:       <move command, or copy then verify then remove>
Collisions:   <none | the existing entry and why it is safe>
References:   <paths that will now point at a retired location>
Tracked:      <yes or no, per path, and whether version control should record the move>
```

Then stop and ask for an explicit yes naming the destination.

## Move

1. Create the destination directory only if its parent already exists and the location was approved.
2. Fail rather than overwrite. If the destination exists, stop and report.
3. Preserve the original content exactly. No reformatting, no renaming of files inside, no edits on the way.
4. If the records are tracked, record the move with version control so the history keeps its shape. If they are not tracked, copy, verify the copy, then remove the source.
5. Never use a bulk delete or a wildcard that could touch anything outside the previewed set.

## Verify after

- Every previewed file exists at the destination.
- The count matches, and the bytes match, not only the names.
- The source paths are gone, and nothing outside the previewed set moved.
- Links and records that pointed at the old location are updated or listed as broken.

## Report

```text
Spec:        <name>
Complete:    <n>/<n> units, verification: <command and result>
Moved:       <source> -> <destination>
Revision:    <spec revision or tag at handoff>
Successor to: <none | exact path to prior archived version>
Count:       <n> files, bytes match
Collisions:  none | <what existed and why nothing was overwritten>
Broken links: none | <the references needing an update>
Untouched:   <everything else>
```

## Rules

- Never archive to clear a task list. Incomplete work is reported, not filed.
- Never overwrite, merge, or "tidy" an existing archive entry.
- Never archive a spec that another spec still depends on.
- Never hide a missing verification behind a summary.
- Never let the archive location become an active instruction set. Retired records are history, not work to do.
- Stop and ask when the destination, ownership, or approval is unclear.

## Done when

The records are at the approved destination, verified by content, nothing was overwritten, and every broken reference is known and named.
