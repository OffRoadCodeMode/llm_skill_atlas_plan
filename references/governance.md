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
