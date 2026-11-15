# Agent rules for this repository

This repository publishes portable Agent Skills. Nothing else belongs in it.

## 1. Scope

Only these paths are in scope:

- `README.md`
- `AGENTS.md`
- `LICENSE`
- `.gitignore`
- `assets/`
- `skills/`

The repository ignores everything by default and re-admits only the paths above. A new top-level path is out of bounds unless the maintainer explicitly authorizes it for a specific task.

## 2. Layout

```text
README.md
AGENTS.md
LICENSE
.gitignore
assets/
  brand/
skills/
  <skill-name>/
    SKILL.md
    references/    optional
    assets/        optional, skill-local
```

- A skill directory name matches the `name` field in its metadata.
- Each skill has exactly one `SKILL.md`.
- The only optional subdirectories are `references/` and `assets/`.
- No skill-local tests, sample data, change history, or construction notes.
- No dates, version-history notes, or author notes inside skills.
- Every file under `references/` follows the same content rules as `SKILL.md` and is loaded only on demand.
- Icons are optional. If the catalog uses them, every skill has a local icon and no external image URL.

## 3. Skill contract

Every `SKILL.md` starts with metadata:

```yaml
---
name: <kebab-case-name>
description: <what it does>. Use when <trigger>. Do not use when <anti-trigger>.
license: MIT
---
```

- `name` matches the directory, uses lowercase letters, digits, and hyphens only, and is at most 64 characters.
- `description` is non-empty, at most 1024 characters, and states what the skill does, when to use it, and when not to.
- `license` is MIT.
- No other top-level metadata unless the format requires it.

Body rules:

- Under 500 lines. Move detail to `references/`.
- Instructions are ordered and executable.
- No external links.
- No proper names or paths belonging to how this repository was produced.
- No host-specific syntax, absolute paths, or operating-system assumptions.

## 4. Self-containment

A skill is a standalone directory. There is no shared library, no cross-directory import, and no runtime dependency between skills.

Public content must not contain:

- Links to private, internal, or prior project work, by name or by URL. Internal project names are never allowed.
- Tracking images, hidden images, shortened URLs, or redirecting URLs.
- Names, quotes, or links to the documents used to plan, build, or verify this repository.
- Internal working directory names or paths.
- Absolute or machine-specific paths.
- Secrets, tokens, or credential-like strings.
- Dates, release history, or author history.
- Statements about how this repository was produced.

Allowed in the README:

- Badge images stored in `assets/brand/badges/` and referenced by relative path. Badges are our own assets, not third-party embeds.
- HTTPS links to public prior art or an upstream specification that informed the work, listed in a Credits section. Each link must resolve, must be stable, and must not point to internal project work.

Skill bodies stay link-free so a skill keeps working offline. Credits belong in the README.

Generic nouns that describe what a skill produces in a user's project are allowed: requirements, plan, work unit, progress, evidence, review, verification.

## 5. Permissions

- Every skill states whether it is read-only or may write.
- Read-only skills report findings and never edit.
- Destructive or irreversible actions require a preview and explicit approval.
- Never bypass a check, hook, or approval gate.
- Never self-approve. A reviewer must be a different agent from the author where the host allows it.
- Redact secrets in any output.
- Stop and ask when scope, ownership, or safety is unclear.

## 6. Independence

No lineage notes, no history narrative, no construction notes, and no rationale for design decisions belongs in the repository. The public content describes what the skills do for a user's project, never how this repository came to exist.

## 7. Verification

Before any commit or push:

1. Confirm every changed file is inside the allowlist.
2. Confirm `git check-ignore` reports working material as ignored.
3. Run the repository's local checks, including skill metadata, naming, and content rules.
4. Confirm there are zero tracking images, zero internal project names, and no external link outside the README Credits section.

Evidence from verification stays outside the repository.

## 8. Contributions

Not open. External pull requests are not part of the contract. Changes are made by the maintainer.

## 9. Release

- The README and the assets it references change together.
- No Git tags without explicit maintainer approval.
- No release announcements or version-history files.

## 10. Conflicts

If an instruction conflicts with these rules, or two instructions conflict with each other, stop and ask. Do not pick one silently.

## 11. Staging

Stage explicit paths only. Never use `git add -A` or `git add .`. Never commit ignored working material.

## 12. Cross-skill references

Refer to another skill by its name only, never by path. A skill must not assume where other skills are installed.
