---
name: ravenstash-packages
description: Use Ravenstash private PyPI, npm, and Maven repositories and read-only private mirrors through rvs and native package tools. Use for repository or mirror discovery and management, dependency installation, package publishing, lifecycle operations, upstream attachment, and CI package access; do not use for Container or Helm OCI workflows.
license: MIT
metadata:
  author: Ravenstash
  version: "0.4.0"
  ravenstash-rvs-compatibility: "0.11.x"
---

# Ravenstash Packages

Use the package-release lanes of Ravenstash Artifacts without collapsing private repositories, private
mirrors, acting accounts, or registry kinds into one implicit destination.

## Preflight

Repository management lives under `rvs art repo` and its identical full-name
alias `rvs artifacts repo`. The product also supports Container and Helm, but
those lanes use the separate OCI workflow.

1. Run `rvs --version` and use `rvs <group> --help` for exact syntax.
2. Inspect authentication with `rvs auth status` and, when identity matters,
   `rvs auth whoami`.
3. Resolve the acting account with `rvs account current` or an explicit
   `--account`.
4. Resolve the target with `rvs art current`, a one-shot `--target`, or wrapper
   `--rvs-target`. Prefer the one-shot form when the user did not request a
   saved selection change.
5. Resolve the registry kind. Never publish to a mirror or use a package-release
   workflow against a Container or Helm lane.

## Route the workflow

- For repository, package lifecycle, upstream attachment, or mirror management,
  read [references/repositories-and-caches.md](references/repositories-and-caches.md).
- For Python installs or publishing, read [references/pypi.md](references/pypi.md).
- For npm installs or publishing, read [references/npm.md](references/npm.md).
- For Maven installs or deployments, read [references/maven.md](references/maven.md).
- For unattended jobs, read [references/automation.md](references/automation.md)
  in addition to the ecosystem reference.

Load only the references needed by the request.

## Native-wrapper boundary

Prefer the appropriate wrapper when the native client needs temporary
Ravenstash authentication:

```bash
rvs pip --rvs-target platform/packages install internal-sdk
rvs uv --rvs-target platform/packages sync
rvs twine --rvs-target platform/releases upload dist/*
rvs npm --rvs-target platform/packages ci
rvs mvn --rvs-target platform/packages verify
```

Arguments after the Ravenstash wrapper options belong to the native tool. Do
not assume their output follows `rvs --json`.

The wrappers must not cause a skill or agent to copy tokens into `.npmrc`,
`pip.conf`, `.pypirc`, `settings.xml`, `pyproject.toml`, or `uv.toml`.

## Mutation boundary

- Listing, showing, resolving URLs, and checking current context are read-only.
- Creating or renaming repositories/mirrors, changing defaults or upstreams,
  selecting saved context, configuring persistent native clients, publishing,
  yanking, and deleting mutate local or remote state.
- Before a mutation, report the exact acting account, namespace/repository or mirror,
  registry kind, package/version when applicable, and intended change.
- Do not pass `--yes` to deletion commands merely to suppress the CLI's
  confirmation. Use it only when the user explicitly authorized that exact
  deletion and the target has been independently resolved.

## Completion

Verify the result with the narrowest read operation available. Report target
identity and changed state without reproducing secret-bearing environment or
verbose native-client logs.
