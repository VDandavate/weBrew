# weBrew

<p align="center">
  <img src="assets/brand/weBrewHero.png" alt="weBrew: from idea to verified software" width="70%" />
</p>

<p align="center">
  <em>Nine portable Agent Skills for the software lifecycle. From a rough idea to reviewed, verified, archived delivery.</em>
</p>

<p align="center">
  <img src="assets/brand/badges/licenseMit.svg" alt="License: MIT" />
  <img src="assets/brand/badges/skillsNine.svg" alt="9 skills" />
  <img src="assets/brand/badges/formatAgentSkills.svg" alt="Agent Skills format" />
  <img src="assets/brand/badges/version.svg" alt="Version 0.1.0" />
</p>

---

## Table of Contents
- [Why weBrew](#why-webrew)
- [What weBrew is](#what-webrew-is)
- [Design principles](#design-principles)
- [The workflow](#the-workflow)
- [Skill catalog](#skill-catalog)
- [Install](#install)
- [Quick start](#quick-start)
- [Worked scenarios](#worked-scenarios)
- [Trust model](#trust-model)
- [How skills load](#how-skills-load)
- [Repository rules](#repository-rules)
- [FAQ](#faq)
- [Credits](#credits)
- [Version](#version)
- [License](#license)

---

## Why weBrew

Agents write code quickly and confidently. They also skip the parts that make software trustworthy: agreeing what "done" means, reviewing the plan before building it, proving the change works, and leaving a record someone else can follow.

The result is a lot of motion and very little confidence. Work that cannot be checked, decisions nobody wrote down, tests that were never run, and a diff that looks finished until it meets production.

weBrew puts those parts back. It is not a framework, a runtime, or a command-line tool. Each skill is a short, self-contained instruction set that an agent loads when the task matches it. Used alone, a skill fixes one habit. Used together, they form a workflow that produces work you can inspect, challenge, and hand over.

## What weBrew is

- **Nine skills**, covering the stages of software work, from shaping an idea to retiring finished records.
- **Plain Markdown.** A skill is a directory containing a `SKILL.md` file with a name, a description, and instructions. Nothing to compile, nothing to run.
- **Self-contained.** No shared library, no cross-skill imports, no dependency between skills.
- **Portable.** Copy or symlink a skill directory into any host that supports the Agent Skills format.
- **Report before writing.** Review, diagnosis, and status skills never edit. Skills that change code do so only inside an approved scope.
- **Spec-driven.** One spec per feature, broken into work units, each with a scope and an acceptance statement.

## Design principles

| Principle | What it means in practice |
|---|---|
| Requirements before code | Nothing is built until the outcome and the acceptance statement are written down and agreed. |
| Review is adversarial | The reviewer hunts for what is wrong. No skill reviews or approves its own output. |
| Evidence over claims | A skill reports the exact command and the exact result, and separates confirmed, suspected, and unverified findings so nothing looks stronger than it is. |
| Approval before destruction | Destructive or irreversible actions need a preview and an explicit yes. |
| The human owns delivery | Agents may prepare and commit on a branch. Merge, push, release, and archival belong to the operator. |
| One writer per area | When several specs run, the build skill refuses to work two that touch the same files. |
| Stop, do not guess | Conflicting instructions, unclear ownership, or a missing approval end the run with a question. |

## The workflow

```text
   IDEA
    │
    ▼
 ┌────────────────────────┐
 │ Grill Ideas to Specs   │  agree the outcome, break it into units
 └───────────┬────────────┘
             ▼
 ┌────────────────────────┐
 │ Review the Specs       │  challenge it before anyone builds it
 └───────────┬────────────┘
             ▼
 ┌────────────────────────┐
 │ Build the Specs        │  one commit and one gate per unit
 └───────────┬────────────┘
             ▼
 ┌────────────────────────┐
 │ Audit and Refine       │  understand the code, rank what to fix
 │ Test and Verify        │  run risk-based checks, report evidence
 │ Assure Quality         │  coordinate the three above
 └───────────┬────────────┘
             ▼
 ┌────────────────────────┐
 │ Archive the Specs      │  retire finished work after verification
 └───────────┬────────────┘
             ▼
        DELIVERED

 At any point:  Diagnose the Bug  ·  Check Status
```

The stages are optional and repeatable. A small change may use one skill. A large build may cycle through the quality loop several times.

## Skill catalog

### Shape the work

| Skill | Use it for |
|---|---|
| Grill Ideas to Specs | Turn a rough idea into agreed requirements, one question at a time. Produces one spec per feature, broken into work units. |
| Review the Specs | Independent read-only review of a spec before it unlocks implementation. Finds ambiguity, missing acceptance, and silent risk. |

### Build

| Skill | Use it for |
|---|---|
| Build the Specs | Find the next eligible spec, or build the one you name, with a failing check, a gate, and a commit for every unit. |

### Assure

| Skill | Use it for |
|---|---|
| Audit and Refine | Map and explain a repo or a path, rank defects and debt, then apply approved cleanups that keep behavior identical. |
| Test and Verify | Run risk-based checks and project gates, then report exact commands and results. |
| Assure Quality | Coordinate audit, refinement, and verification without self-approval. |

### Operate

| Skill | Use it for |
|---|---|
| Diagnose the Bug | Find the root cause with a tight reproduction loop, and leave a failing regression test. |
| Archive the Specs | Retire a completed spec after verification, without overwriting an earlier archive. |
| Check Status | Read-only report of spec state, evidence, ownership, and blockers. |

## Install

Copy or symlink the skill directories you want into your host's skill directory. A skill is a directory containing `SKILL.md` with `name` and `description` metadata.

One skill:

```bash
mkdir -p <your-host-skill-directory>
cp -R skills/build-the-specs <your-host-skill-directory>/
```

All nine:

```bash
cp -R skills/* <your-host-skill-directory>/
```

Link instead of copy, so local edits stay live:

```bash
ln -s "$PWD/skills/build-the-specs" <your-host-skill-directory>/build-the-specs
```

Confirm discovery by listing what the host reports. A skill that loads without a description has a malformed metadata block.

## Quick start

```text
You:    I want to add saved searches to the dashboard, but I have not
        thought it through.

Agent:  [Grill Ideas to Specs]
        What changes for the user when saved searches work?
        A) They can re-run a search without retyping it  (recommended)
        B) They share a search with a teammate
        C) They get notified when new results match
```

The skill asks one question at a time, marks a recommendation, and records the answer. When the outcome, boundary, non-goals, failure path, and acceptance are settled, it writes one spec per feature, each with work units in dependency order. Then it waits for approval.

From there:

```text
Review the Specs   →  "not ready: two acceptance statements are untestable"
Build the Specs    →  finds the spec still has 3 of 7 units open, builds it
Archive the Specs  →  after every unit is verified and you approve
```

## Worked scenarios

**A feature with several units.** Grill Ideas to Specs writes one spec for it. Review the Specs challenges it. Build the Specs counts what remains, orders the units, and lands one commit and one gate result per unit, recording as it goes.

**Two specs in flight.** Build the Specs builds the one that unblocks the other first, and refuses to run both when their units touch the same files.

**A defect with an unknown cause.** Diagnose the Bug builds a failing signal, bisects, confirms the cause with evidence, and writes a regression test. The fix goes through a spec, so it is built and recorded like anything else.

**A repository nobody has touched in a year.** Audit and Refine maps what the code does, tests its executable claims, and ranks what is stale, duplicated, or missing. Approved cleanups are applied one commit at a time, with behavior unchanged.

**A quality pass before release.** Assure Quality dispatches audit, refinement, and verification, then requires an independent verifier before anything is called done.

## Trust model

- **Reports first.** Review, diagnosis, and status skills never edit. They report.
- **Approval gates.** Destructive or irreversible actions need a preview and an explicit yes.
- **No self-approval.** A reviewer is a different agent from the author wherever the host allows it. A skill never approves its own output.
- **Explicit staging.** Commits stage named paths. No skill uses `git add -A` or `git add .`.
- **No hidden pushes.** Skills may commit on a branch. Merge, push, tag, release, and archival stay with the operator.
- **Honest evidence.** Confirmed, suspected, and unverified findings are labeled separately, and findings are ranked P0 to P3.

## How skills load

Loading happens in three tiers: the host reads only `name` and `description` at discovery, loads the full `SKILL.md` when a request matches the description, and reads `references/` or `assets/` only if the instructions call for them.

That is why each description states three things: what the skill does, when to use it, and when not to. The third clause is what keeps the wrong skill from firing.

If your host supports explicit invocation, call the skill by name. If it does not, describe the work and let the description match. There is no `webrew` command and no launcher.

## Repository rules

The repository tracks only its allowlist: `README.md`, `AGENTS.md`, `LICENSE`, `.gitignore`, `assets/`, and `skills/`. Everything else is ignored by default, and a new top-level path needs explicit approval. Agent-facing rules, including the skill contract and the permission model, are in [`AGENTS.md`](AGENTS.md).

## FAQ

**Do I need all nine?**
No. Each skill stands alone. Most people start with Grill Ideas to Specs and Build the Specs.

**Will it commit to my branch?**
The build skill commits explicit paths on the current branch. Audit and Refine commits only approved cleanups. Nothing pushes, merges, or tags without you.

**Can two specs be built at once?**
Only when their units do not touch the same files. Build the Specs refuses the collision rather than resolving it silently.

**What if the spec is wrong?**
Review the Specs exists for that. It returns ready, ready with changes, or not ready, with findings and evidence.

## Credits

- The Agent Skills format and its specification: [agentskills.io/specification](https://agentskills.io/specification)
- README structure informed by [Awesome README](https://github.com/matiassingers/awesome-readme)

The skill set itself is original work.

## Version

0.1.0

## License

MIT. See [`LICENSE`](LICENSE).
