# Authentication and context

Load this reference for interactive login, automation authentication, multiple
profiles, acting-account selection, or package-target resolution.

## Interactive login

```bash
rvs auth login
rvs auth login --profile work
rvs auth login --duration 8h
rvs auth status
rvs auth whoami
```

Device login requires the user to review and approve the browser request. Do
not automate that approval. The server may reduce a requested duration to its
current policy.

Manage profiles without exposing credentials:

```bash
rvs auth profile list
rvs auth profile switch work
rvs auth profile rename old-name new-name
rvs auth logout
```

Profile switching is persistent local state. Use `--profile` or wrapper
`--rvs-profile` for a one-shot operation instead.

## Automation

An unattended process receives its token through the environment:

```bash
export RVS_TOKEN="${TOKEN_FROM_SECRET_STORE}"
rvs auth whoami
```

The value must originate in the user's secret manager. Never put it in a skill,
repository, command transcript, shell profile, or `~/.rvs/config.toml`.

## Acting account

```bash
rvs account list
rvs account current
rvs account switch personal
rvs account switch org:acme
```

An account switch is persistent for the active profile. For one operation, use
the command's `--account` option or a wrapper's `--rvs-account` option.

## Package target

```bash
rvs pkg current
rvs pkg select platform/backend
rvs pkg clear
```

Target forms are `workspace/repository`, `cache:source`, and
`custom-cache:name`. Prefer one-shot `--target` or wrapper `--rvs-target` when
the user did not ask to change saved selection.

Resolution precedence is:

1. Explicit one-shot target.
2. Selected target for the active profile and account.
3. Legacy per-kind private-repository default.
4. For supported reads, the account's enabled official cache.

An explicit account is resolved before the target. A failed authorization or
missing immutable target must not trigger a retry against another account or a
same-named repository.
