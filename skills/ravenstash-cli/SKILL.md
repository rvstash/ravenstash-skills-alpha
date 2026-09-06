---
name: ravenstash-cli
description: Install, update, authenticate, inspect, and troubleshoot the Ravenstash rvs CLI, including local profiles, acting accounts, artifact targets, shell integration, structured output, and managed Python, Node.js, or Java runtimes. Use for rvs setup and local CLI context; use a registry workflow skill for package or OCI operations.
license: MIT
metadata:
  author: Ravenstash
  version: "0.4.0"
  ravenstash-rvs-compatibility: "0.11.x"
---

# Ravenstash CLI

Use `rvs` for Ravenstash login, local context, repository discovery, and
temporary native-tool authentication. Native tools still own package formats,
lock files, builds, and dependency resolution.

## Preflight

1. Check for the CLI with `command -v rvs`.
2. If present, run `rvs --version`. This skill targets the 0.11 compatibility
   channel; if the installed command differs, use its help and current release
   notes rather than forcing these examples.
3. Discover exact syntax with `rvs --help` and `rvs <group> --help`. Never guess
   a subcommand or option.
4. Before authenticated work, inspect `rvs auth status`, `rvs auth whoami`,
   `rvs profile current`, `rvs account current`, and, when the complete tuple
   matters, `rvs context current`. For package work, use `rvs art current` as
   needed.

For installation, signed updates, runtime management, or shell changes, read
[references/install-update-runtime.md](references/install-update-runtime.md).
For local profiles, acting accounts, and target precedence, read
[references/auth-and-context.md](references/auth-and-context.md).

## Structured output

Place the global flag before the command:

```bash
rvs --json auth status
rvs --json account current
rvs --json art repo list
```

Ravenstash-owned output is newline-delimited JSON. Output from native
passthrough tools retains the native tool's format. Do not move `--json` after
the command or assume native output becomes JSON.

## Authentication boundary

- Interactive sessions use `rvs auth login` and browser-backed device
  authorization. Do not request a Ravenstash password.
- Verify server identity with `rvs auth whoami`; do not infer it only from local
  metadata.
- Credentials belong in the operating-system keyring. Do not inspect or copy
  them from storage.
- Unattended jobs use `RVS_TOKEN` supplied by a secret manager. Do not run
  device login in CI, write the token to project files, or print it.
- `RVS_TOKEN` takes precedence over profile credentials and is not refreshed.

## Context safety

- Keep authenticated user, local profile, acting account, target, and registry
  kind separate.
- Prefer one-shot `--profile`, `--account`, and `--target` options when the user
  asks for one operation in a different context. Do not persist a switch unless
  requested.
- If a selector is ambiguous, stop and present the stable choices. Never choose
  by name similarity.
- Runtime shell shims change a startup file and require user intent. The former
  context-prompt `rvs shell` commands are removed; do not reinstall them.

## Unsupported groups

`rvs art` and `rvs artifacts` invoke the same product group, including its `repo`
subgroup. Top-level `rvs repo` is absent. Do not teach the temporary hidden
`rvs pkg` spelling; older installations need a supported update, not a fallback.
`rvs ci` remains a registered placeholder. Do not invent CI subcommands. CI package access uses
an automation token and the documented package-manager or `rvs` workflow.

## Completion

Report the installed version and effective local profile, acting account, and
target when relevant, the
verification command used, and any local files or shell configuration changed.
Never include credential material.
