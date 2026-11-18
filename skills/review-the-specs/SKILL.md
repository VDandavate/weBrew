---
name: review-the-specs
description: Independent adversarial review of requirements and plan before build. Use when a plan or requirement set is proposed, revised, or about to unlock implementation. Do not use when reviewing code, or when the user wants fixes rather than findings.
license: MIT
---

# Review the Specs

Find what will hurt before anyone writes code.

## When to use

- A plan or requirement set is proposed or revised.
- A plan is about to unlock implementation.
- The user asks whether a plan is sound, complete, or safe.

## When not to use

- The subject is code. Use Review the Code.
- The user wants changes, not findings. Report first, then hand off.
- Requirements do not exist yet. Use Grill Ideas to Specs.

## Permissions

Read-only. Reports findings, questions, and a verdict. Never edits the plan.

## Rules

State your independence before you judge. Reviewing your own plan is the weakest form of review, so arrange one of these first:

1. Several agents available: dispatch a dedicated reviewer with read-only tools, given only the plan text and the original request. No summary, and no defense of the plan.
2. One agent, one session: isolate the review into a separate turn. Clear the assumptions from earlier conversation, adopt the explicit persona of an adversarial auditor, and judge strictly from the written text.
3. Neither possible: state `Independence: limited (single-session auditor review)` and ask the operator to confirm before anyone relies on the verdict.

If you authored the plan, say so and mark every finding as lower confidence.

- Cite the text you are judging. A finding without a quote or a location is an opinion.
- Separate three tiers: confirmed from the text, suspected, and unverified.
- Rank findings P0 to P3. P0 means building on this produces a wrong or unsafe outcome.
- Ask about gaps you cannot resolve from the text instead of assuming.
- Never soften a finding to be agreeable, and never inflate one to look thorough.
- If the plan is sound, say so plainly and stop.

## Process

1. Get the plan and the request it came from. If the request is missing, that is the first finding.
2. Check completeness against this list: outcome, scope, non-goals, users, constraints, dependencies, acceptance, failure and rollback, security and data handling, ownership.
3. Check ambiguity. Flag any sentence that two competent people would read differently. Rewrite it in the finding as the two readings.
4. Check testability. For each acceptance statement, ask whether it can be verified without judgement. Untestable acceptance is a P1.
5. Check scope. Find work that the outcome does not need, and work the outcome implies but the plan omits.
6. Check order. Find a unit that depends on something later in the sequence.
7. Check failure paths. If this ships broken, what detects it and what undoes it? Silence here is a P1.
8. Check risk. Trust boundaries, destructive operations, secrets, personal data, irreversible migrations, third-party availability.
9. Check ownership. Name who is responsible for each unit and who reviews it. Self-review is not review.
10. Write the findings. Sort by severity. For each: what, where, why it matters, what to do.
11. Give a verdict: ready, ready with changes, or not ready.

## Output

- Verdict, in one line, at the top.
- Findings by severity with evidence.
- Open questions the plan cannot answer on its own.
- What you did not review, and why.

## Done when

The verdict is stated, every P0 and P1 has a location and a reason, and the remaining gaps are visible to the reader. A review that ends without a verdict is unfinished.
