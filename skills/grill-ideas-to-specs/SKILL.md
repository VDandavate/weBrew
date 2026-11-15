---
name: grill-ideas-to-specs
description: Turn a rough idea into agreed requirements and an approved plan. Use when the idea is vague, scope is unclear, or acceptance criteria are missing. Do not use when requirements are already agreed and documented, or when the user asked for implementation.
license: MIT
---

# Grill Ideas to Specs

Turn an idea into something that can be built and checked.

## When to use

- The request is a sentence or two and cannot be built from it.
- Two people would build different things from the same description.
- Nobody has written down what "done" means.

## When not to use

- Requirements are agreed and recorded; move to planning or building.
- The user asked for code now. Say what is missing and ask whether to proceed anyway.
- The task is a single obvious change with a clear outcome.

## Permissions

May write documentation. Never writes production code. Writes only after explicit approval of the plan.

## Rules

- Ask one question at a time. Never send a list of questions.
- Every question carries options and a marked recommendation.
- Never invent a requirement. If the answer is missing, record the gap.
- When the user says "not sure", recommend the conservative default and record the risk of that default.
- Reflect each answer back in one or two sentences before moving on.
- No implementation, no scaffolding, and no dependency choice before approval.
- Refer to other skills by name only.

## Process

1. Read the project rules and the existing documentation. Find where this project keeps its requirements. If no location is obvious, ask.
2. State your understanding of the idea in three sentences. Correct it with the user before asking anything else.
3. Question in this order, one at a time:
   1. Outcome. What changes for the user when this works?
   2. Boundary. What is inside this piece of work?
   3. Non-goals. What is deliberately outside it?
   4. Constraints. Platform, compatibility, performance, cost, deadline.
   5. Dependencies. What must exist or be decided first.
   6. Failure. What breaks, how it is detected, how it is undone.
   7. Acceptance. How will we know it is done, without asking anyone's opinion?
   8. Risk. What is uncertain, irreversible, or security-relevant.
4. Stop questioning when each of those eight has an answer or a recorded gap.
5. Propose one spec per feature. Break each spec into work units in dependency order, small enough to verify on their own, each with a testable acceptance statement. If the idea spans several features, say so and write one spec for each.
6. Show the plan and the open gaps. Ask for approval.
7. On approval, write the agreed requirements and plan where the project keeps its documentation, in the project's existing format.
8. Stop. Hand off to Review the Specs, or to the build controller if the project has one.

## Output

- The agreed outcome in plain language.
- Scope, non-goals, constraints, and dependencies.
- One spec per feature, each with units and acceptance statements.
- Open questions and the risk of each remaining gap.
- Approval state: approved, or waiting.

## Done when

Every unit has an acceptance statement that a different person could check, the scope has explicit edges, and approval is recorded. If any of those is missing, the work is not done.
