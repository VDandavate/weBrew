# weBrew

<p align="center">
  <img src="assets/brand/weBrewHero.png" alt="weBrew: from idea to verified software" width="100%" />
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
  <a href="#why">Why</a> ·
  <a href="#how">How</a> ·
  <a href="#what">What</a> ·
  <a href="#install">Install</a> ·
  <a href="#quick-start">Quick start</a> ·
  <a href="#faq">FAQ</a> ·
  <a href="#credits">Credits</a> ·
  <a href="#license">License</a>
</p>

Agents write code fast. They skip the parts that make software trustworthy: agreeing what "done" means, reviewing before building, proving the change works, leaving a record someone else can follow.

weBrew puts those parts back. Each skill is a short, self-contained instruction set an agent loads when the task matches. No framework, no runtime, no CLI. Copy or symlink the directories you want into your host's skill directory.

## Why

Three failures make agent output hard to trust.

| Problem | What happens | What weBrew does |
|---|---|---|
| No shared definition of done | Agents build past acceptance, nothing is verifiable | One spec per feature with work units and acceptance |
| Review happens after the build | Rework is expensive, problems surface late | Read-only adversarial review before anything is built |
| No record of what changed | Diffs look finished until production | Evidence, approvals, and a trail someone else can follow |

## How

Each task runs through phases.

| Phase | Skills | What you get |
|---|---|---|
| Specify | Grill Ideas to Specs, Review the Specs | An approved spec with work units |
| Build | Build the Specs | Built units, tested and committed |
| Assure | Audit and Refine, Test and Verify, Assure Quality | Ranked findings and independent verification |
| Deliver | Archive the Specs | Records archived without overwriting |

The Assure phase repeats until you are satisfied. Diagnose the Bug when something breaks. Check Status when you need to know where things stand.

Trust model:

- Reports first. Review, diagnosis, and status skills never edit. They report.
- Approval gates. Destructive or irreversible actions need a preview and an explicit yes.
- No self-approval. A reviewer is a different agent from the author wherever the host allows it. A skill never approves its own output.
- Explicit staging. Commits stage named paths. No skill uses `git add -A` or `git add .`.
- No hidden pushes. Skills may commit on a branch. Merge, push, tag, release, and archival stay with the operator.
- Honest evidence. Confirmed, suspected, and unverified findings are labeled separately. Findings ranked P0 to P3.

## What

Nine portable skills. The directory name is the skill name you copy or link.

| Skill | Skill name | Does | Reach for it when |
|---|---|---|---|
| Grill Ideas to Specs | `grill-ideas-to-specs` | Turns a vague idea into an approved spec, one question at a time | You know what you want but not how to specify it |
| Review the Specs | `review-the-specs` | Read-only adversarial review of a spec | A spec looks ready and you want problems found first |
| Build the Specs | `build-the-specs` | Builds one spec unit by unit: test first, gate, commit, record | You have approved specs and open units |
| Audit and Refine | `audit-and-refine` | Maps code, ranks defects and debt, applies approved cleanups | Code needs a deep quality read |
| Test and Verify | `test-and-verify` | Risk matrix, cheapest checks first, labeled evidence | A change needs verification before it is done |
| Assure Quality | `assure-quality` | Coordinates assess, propose, approve, dispatch, verify, reconcile; never self-approves | A quality pass needs multiple specialists |
| Diagnose the Bug | `diagnose-the-bug` | Failing signal, bisect, evidence, regression test | Something is broken and the cause is unknown |
| Archive the Specs | `archive-the-specs` | Verifies completion, previews the move, archives without overwriting | Every unit is complete and records should be retired |
| Check Status | `check-status` | Reads Git, specs, approvals, evidence; reports state; changes nothing | You need to know where things stand |

## Install

Copy or symlink the skill directories you want into your host's skill directory. A skill is a directory containing `SKILL.md` with `name` and `description` metadata. Use the directory names in the Skill name column above.

| Scope | Command |
|---|---|
| One skill | `cp -R skills/<name> <your-host-skill-directory>/` |
| All nine | `cp -R skills/* <your-host-skill-directory>/` |
| Link instead | `ln -s "$PWD/skills/<name>" <your-host-skill-directory>/<name>` |

Confirm discovery by listing what the host reports. A skill that loads without a description has a malformed metadata block.

## Quick start

```
You:    I want to add saved searches to the dashboard, but I haven't
        thought it through.

Agent:  [Grill Ideas to Specs]
        What changes for the user when saved searches work?
        A) They can re-run a search without retyping it  (recommended)
        B) They share a search with a teammate
        C) They get notified when new results match
```

The skill asks one question at a time, marks a recommendation, and records the answer. When outcome, boundary, non-goals, failure path, and acceptance are settled, it writes one spec per feature with work units in dependency order. Then it waits for your approval. Next comes review, build, and archive.

## FAQ

**Do I need all nine?** No. Each skill stands alone. Most start with Grill Ideas to Specs plus Build the Specs.

**Two specs at once?** Only if units do not share files. Build the Specs refuses collisions.

## Credits

- Agent Skills format: [agentskills.io/specification](https://agentskills.io/specification)
- README inspiration: [Awesome README](https://github.com/matiassingers/awesome-readme)

Skill set is original work.

## License

MIT: see [`LICENSE`](LICENSE).

Version 0.1.0.

<p align="center"><a href="#webrew">Back to top</a></p>
