# Governance — Modifying Atlas

Atlas is a **standard**, not a per-project scratchpad. These rules keep it coherent
as it evolves.

## Rules

1. **Atlas is a standard** — don't modify it for project-specific needs. Put
   project-specific content in the project's `project/` folder or its `AGENTS.md`.
2. **Modifications must be justified** — a change to the skill should apply
   broadly, not just to the project in front of you.
3. **Log notable changes** — record what changed and why in `references/changelog.md`;
   bump the `version:` field in `SKILL.md` frontmatter for convention changes.
4. **Project-specific needs stay in the project** — if only one project wants a
   convention, it lives in that project's content, never in the skill.
5. **Domain packs are shareable** — a custom pack (e.g. `van-conversion`) helps
   whoever plans a similar project; prefer adding a pack over bending the core.
   See "Creating a custom domain pack" below.

## Versioning

The skill's version (semver; first release `0.1.0`) lives in the `SKILL.md`
frontmatter `version:` field. Each project **stamps the version it was
onboarded under** into its own metadata — a line in the root `AGENTS.md` and the
`project/DASHBOARD.md` footer, e.g. `atlas: 0.1.0`. This means:

- a project records which conventions shaped it, and `audit` can note when the
  installed skill has moved on ("onboarded under 0.1.0, skill is now 0.3.0 —
  conventions may have changed");
- breaking convention changes are **major** bumps, so an older project knows which
  rules applied when it was built.

There are **no `+local` suffixes or fork bookkeeping** — there is one skill,
edited directly and versioned in git.

## Maintaining the skill (manual)

The skill is **finalised for day-one use and improved over time manually**, like
any markdown-in-git project. There is deliberately **no autonomous self-editing,
no local/upstream tiers, and no PR pipeline** — that machinery made sense for a
heavyweight vendored framework, not a single skill folder a human owns.

The workflow:

1. You (or Hermes, at your direction) notice friction — a template field that's
   always deleted, a confusing convention, a missing agent behaviour.
2. Edit the relevant skill file directly, commit with the `atlas:` prefix, and
   note anything significant in `references/changelog.md` (bump the `version:` field
   in `SKILL.md` frontmatter for convention changes).
3. On the next session the change is live — the agent reads the installed skill.

**Hermes's role is advisory.** It may *surface* recurring friction
("you keep deleting this field — want to drop it from the template?"), but it does
**not** modify the skill on its own. Keeping a human in the loop matches
*prove before scaling* and stops the framework drifting without oversight.

## Creating a custom domain pack

A custom domain pack tailors Atlas to a specific kind of project (e.g.
`van-conversion`, `event-planning`, `academic-research`). The core framework
works for any project, but a pack adds domain-specific phases, system types,
relationship types, templates, and agents.

### When to create one

**Not during onboarding.** Start with the closest shipped pack (`software/` or
`general/`). A custom pack is worth creating only when:

1. The project clearly does not fit either shipped pack.
2. The LLM has enough context (past onboarding, at least one system emerged) to
define meaningful system types and phases for the domain.
3. The user agrees it is worth the effort.

This follows the *prove before scaling* principle: use the generic pack first,
feel the friction, then invest in a custom pack when the need is clear.

### How to create one

1. **Copy** `references/domains/general/` to `references/domains/<name>/` as a
   starting point.
2. **Customize** `phases.md` (domain-specific phases + exit criteria),
   `system-types.md` (system types + relationship types for this domain), and
   add `templates/` or `agents/` subfolders as needed.
3. **Commit** with the `domain:` prefix, e.g. `domain: add van-conversion pack`.
4. **Note** the new pack in `references/changelog.md`.

### Contributing a pack back upstream

If your custom pack could help others planning similar projects, submit a PR to
the Atlas repo:

1. Fork the repo and create a branch (e.g. `domain/van-conversion`).
2. Add your pack under `references/domains/<name>/`.
3. Update `references/changelog.md` with a one-liner.
4. Open a PR with the `domain:` prefix in the title.

Packs should be **self-contained** (own phases, types, templates) and **not
modify the core framework**. The maintainers will review for fit and consistency.
