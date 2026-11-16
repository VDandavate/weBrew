# weBrew

<p align="center">
  <img src="assets/brand/weBrewHero.png" alt="weBrew: from idea to verified software" width="70%" />
</p>

<p align="center">
  <img src="assets/brand/weBrewAvatar.png" alt="weBrew avatar" width="80" />
</p>

<p align="center">
  <b>Nine portable Agent Skills for the software lifecycle.</b><br/>
  From a rough idea to reviewed, verified, archived delivery.
</p>

<p align="center">
  <img src="assets/brand/badges/license.shields.svg" alt="license: MIT" />
  <img src="assets/brand/badges/skills9.shields.svg" alt="skills: 9" />
  <img src="assets/brand/badges/format.shields.svg" alt="format: Agent Skills" />
  <img src="assets/brand/badges/version.shields.svg" alt="version: 0.1.0" />
  <br/>
  <img src="assets/brand/badges/portable.shields.svg" alt="portable: yes" />
</p>

<p align="center">
  <a href="#the-problem">The problem</a> ·
  <a href="#the-workflow">The workflow</a> ·
  <a href="#the-nine-skills">The nine skills</a> ·
  <a href="#install">Install</a> ·
  <a href="#quick-start">Quick start</a> ·
  <a href="#trust-model">Trust model</a> ·
  <a href="#faq">FAQ</a> ·
  <a href="#credits">Credits</a> ·
  <a href="#license">License</a>
</p>

Agents write code fast. They also skip the parts that make software trustworthy: agreeing what "done" means, reviewing the plan before building it, proving the change works, leaving a record someone else can follow.

Your agent can build a feature in an afternoon. weBrew helps you decide whether it should exist at all.

weBrew puts those parts back. Each skill is a short, self-contained instruction set an agent loads when the task matches. No framework, no runtime, no CLI. Copy or symlink the directories you want into your host's skill directory.

---

## 🧩 The problem

| Problem | What happens | What weBrew does |
|---|---|---|
| No shared definition of done | Agents build past acceptance. Nothing is verifiable. | One spec per feature, with work units and acceptance. |
| Review happens after the build | Rework is expensive. Problems surface late. | Read-only adversarial review before anything is built. |
| No record of what changed | Diffs look finished until production. | Evidence, approvals, and a trail someone else can follow. |

---

## 🔄 The workflow

```
Idea
  ↓
🔥 Grill Ideas to Specs → 🔍 Review the Specs → 🏗️ Build the Specs
                                              ↓
                                    ┌─────────────────────────┐
                                    │   QUALITY LOOP          │
                                    │  🧭 Audit and Refine    │
                                    │  🧪 Test and Verify     │
                                    │  🎯 Assure Quality      │
                                    └─────────────────────────┘
                                              ↓
                                    📦 Archive the Specs → ✅ Delivered
```

At any point: 🐛 Diagnose the Bug · 📊 Check Status

Stages are optional and repeatable. A small change uses one skill. A large build cycles the quality loop until you are satisfied.

---

## 📋 The nine skills

| Skill | Does | Reach for it when |
|---|---|---|
| 🎨 **Grill Ideas to Specs** | Turns a vague idea into an approved spec, one question at a time. | You know what you want but not how to specify it. |
| 🔍 **Review the Specs** | Read-only adversarial review of a spec. | A spec looks ready and you want problems found first. |
| 🏗️ **Build the Specs** | Builds one spec unit by unit: test first, gate, commit, record. | You have approved specs and open units. |
| 🧭 **Audit and Refine** | Maps code, ranks defects and debt, applies approved cleanups. | Code needs a deep quality read. |
| 🧪 **Test and Verify** | Risk matrix, cheapest checks first, labeled evidence. | A change needs verification before it is done. |
| 🎯 **Assure Quality** | Coordinates assess → propose → approve → dispatch → verify → reconcile. Never self-approves. | A quality pass needs multiple specialists. |
| 🐛 **Diagnose the Bug** | Failing signal, bisect, evidence, regression test. | Something is broken and the cause is unknown. |
| 📦 **Archive the Specs** | Verifies completion, previews the move, archives without overwriting. | Every unit is complete and records should be retired. |
| 📊 **Check Status** | Reads Git, specs, approvals, evidence. Reports state. Changes nothing. | You need to know where things stand. |

<details>
<summary>Full skill descriptions</summary>

**Grill Ideas to Specs**: Turns a vague idea into an approved spec, one question at a time. Marks a recommendation each turn. Writes one spec per feature with work units in dependency order, then waits for your approval.

**Review the Specs**: Read-only adversarial review. Finds ambiguity, untestable acceptance, scope creep, and dependency gaps. Returns ready, ready with changes, or not ready, with evidence.

**Build the Specs**: Picks the next eligible spec, or the one you name. Builds it unit by unit. One spec per run. Hard stop. Refuses to run two specs whose units touch the same files.

**Audit and Refine**: Phase 1 audits: maps the code, states the model, runs safe checks, ranks defects (bugs, security, data loss, scale) and debt (dead code, duplication, speculation, standards breaks) P0 to P3. Phase 2 refines: applies approved cleanups one commit at a time, behavior proven unchanged.

**Test and Verify**: Builds a risk matrix. Runs the cheapest discriminating checks first. Records exact commands and results. Classifies findings as confirmed defect, suspected risk, coverage gap, unverified, or not run.

**Assure Quality**: Coordinates the pass: assess, propose batches, wait for approval, dispatch, require independent verification, reconcile. Never self-approves.

**Diagnose the Bug**: Builds a failing signal, minimizes, bisects, tests hypotheses, confirms the cause with evidence, leaves a red regression test, and hands off the fix scope.

**Archive the Specs**: Verifies completion. Previews the move. Gets approval. Archives without overwriting. Updates broken references.

**Check Status**: Reads Git, specs, approvals, and evidence. Reconciles contradictions. Reports complete, in progress, blocked, unverified, or not started. Changes nothing.

</details>

---

## 📦 Install

Copy or symlink the skill directories you want into your host's skill directory. A skill is a directory containing `SKILL.md` with `name` and `description` metadata.

`<name>` is the skill's directory name: `archive-the-specs`, `assure-quality`, `audit-and-refine`, `build-the-specs`, `check-status`, `diagnose-the-bug`, `grill-ideas-to-specs`, `review-the-specs`, `test-and-verify`.

| Scope | Command |
|---|---|
| One skill | `cp -R skills/<name> <your-host-skill-directory>/` |
| All nine | `cp -R skills/* <your-host-skill-directory>/` |
| Link instead | `ln -s "$PWD/skills/<name>" <your-host-skill-directory>/<name>` |

Confirm discovery by listing what the host reports. A skill that loads without a description has a malformed metadata block.

---

## 🚀 Quick start

```
You:    I want to add saved searches to the dashboard, but I haven't
        thought it through.

Agent:  [Grill Ideas to Specs]
        What changes for the user when saved searches work?
        A) They can re-run a search without retyping it  (recommended)
        B) They share a search with a teammate
        C) They get notified when new results match
```

The skill asks one question at a time, marks a recommendation, and records the answer. When outcome, boundary, non-goals, failure path, and acceptance are settled, it writes one spec per feature with work units in dependency order. Then it waits for your approval.

From there:

```
Review the Specs   →  "not ready: two acceptance statements are untestable"
Build the Specs    →  finds the spec has 3 of 7 units open, builds it
Archive the Specs  →  after every unit is verified and you approve
```

---

## 🛡️ Trust model

- **Reports first.** Review, diagnosis, and status skills never edit. They report.
- **Approval gates.** Destructive or irreversible actions need a preview and an explicit yes.
- **No self-approval.** A reviewer is a different agent from the author wherever the host allows it. A skill never approves its own output.
- **Explicit staging.** Commits stage named paths. No skill uses `git add -A` or `git add .`.
- **No hidden pushes.** Skills may commit on a branch. Merge, push, tag, release, and archival stay with the operator.
- **Honest evidence.** Confirmed, suspected, and unverified findings are labeled separately. Findings ranked P0 to P3.

---

<details>
<summary>How skills load</summary>

Loading happens in three tiers:

1. **Discovery**: host reads only `name` and `description`
2. **Match**: host loads the full `SKILL.md` when a request matches the description
3. **Deep**: `references/` or `assets/` read only if instructions call for them

That is why each description states what the skill does, when to use it, and when not to. The third clause keeps the wrong skill from firing.

If your host supports explicit invocation, call the skill by name. If not, describe the work and let the description match. There is no `webrew` command and no launcher.

</details>

<details>
<summary>Repository rules</summary>

This repo tracks only its allowlist: `README.md`, `AGENTS.md`, `LICENSE`, `.gitignore`, `assets/`, and `skills/`. Everything else is ignored by default. A new top-level path needs explicit approval.

Agent-facing rules cover the skill contract, permission model, naming, and verification. They are in `AGENTS.md`.

</details>

---

## ❓ FAQ

**Do I need all nine?** No. Each skill stands alone. Most start with Grill Ideas to Specs plus Build the Specs.

**Will it commit?** Yes, explicit paths only. No pushes, merges, or tags without you.

**Two specs at once?** Only if units do not share files. Build the Specs refuses collisions.

**Spec is wrong?** Review the Specs returns ready, ready with changes, or not ready, with evidence.

---

## 🏷️ Credits

- Agent Skills format: [agentskills.io/specification](https://agentskills.io/specification)
- README inspiration: [Awesome README](https://github.com/matiassingers/awesome-readme)

Skill set is original work.

---

## 📄 License

MIT: see [`LICENSE`](LICENSE).

Version 0.1.0.

<p align="center"><a href="#webrew">Back to top</a></p>
