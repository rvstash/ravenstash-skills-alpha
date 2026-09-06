# AGENTS.md

Operational guidance for the Ravenstash Agent Skills repository.

## Repository role

- This is the canonical source for portable `SKILL.md` workflows that teach AI
  agents how to use released Ravenstash products and the `rvs` CLI.
- It is not the public product documentation, a copy of the CLI command
  reference, an MCP server, or an authorization layer.
- Skills live under `skills/<name>/SKILL.md`. Conditional detail belongs in
  that skill's `references/` directory.

## Sources of truth

Before changing product or command claims, verify them against:

1. The released `rvs` binary and its `--help` output.
2. <https://docs.ravenstash.com/> and its `llms.txt` entrypoint.
3. The owning Ravenstash source repository when preparing a coordinated
   unreleased change.

Do not copy private architecture, internal service routes, environment hosts,
customer data, or unreleased credentials into this repository. Examples use
public hosts, documentation-safe names, and obvious placeholders.

## Skill authoring

- Follow the Agent Skills specification: lowercase hyphenated directory/name,
  concise discriminating description, and progressive disclosure.
- Keep each workflow skill self-contained. It may recommend another installed
  skill, but it must not fail merely because a sibling directory was not
  installed.
- Encode Ravenstash-specific decisions, safety invariants, failure boundaries,
  and success checks. Link to public docs instead of copying an exhaustive
  manual.
- Preserve user scope. A skill never turns a read request into a create,
  publish, switch, configure, yank, or delete operation.
- Require the exact local profile, acting account, target, registry kind, and
  object identity where ambiguity could affect another resource.
- Never include token values or instructions to persist credentials.
- In the supported 0.11.x channel, `rvs art repo` and `rvs artifacts repo`
  manage artifact repositories; top-level `rvs repo` is absent and `rvs ci`
  remains a placeholder. Do not invent CI subcommands or a source-repository
  product.
- Do not add skills for roadmap products until a released customer workflow
  gives the skill something concrete to operate or interpret.

## Versioning

- Skill releases use independent semantic versioning.
- Keep `metadata.version` consistent across all files changed for one release.
- State the supported `rvs` compatibility channel in operational skill
  frontmatter.
- A CLI command-surface change requires coordinated skill review before the
  affected release is considered supported.

## Verification

Run from this repository:

```bash
python3 scripts/validate.py
git diff --check
```

Also exercise changed command examples against the pinned compatible `rvs`
binary. Model evals must judge behavior and side effects, not merely wording.
