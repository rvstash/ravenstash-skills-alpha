# Authentication and context

Load this reference for interactive login, automation authentication, multiple
local profiles, acting-account selection, or artifact-target resolution.

Keep four layers distinct:

- the authenticated **user** is the audit actor;
- a **local profile** is named CLI configuration and a credential slot;
- the **acting account** is the personal or organization authorization and
  metering boundary; and
- the **artifact target** is a repository or private mirror inside that account.

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

Inspect and manage local profiles without exposing credentials:

```bash
rvs profile list
rvs profile current
rvs profile use work
rvs profile rename old-name new-name
rvs profile delete old-name
rvs auth logout
```

Profile selection updates the persisted default. Use `--profile` or wrapper `--rvs-profile` for a
one-shot operation instead. `rvs profile current` reports the selection source.

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
rvs account use Avery
rvs account use AcmeHQ
rvs context current
```

Use the Ravenstash username for a personal account or the public handle for an
organization. Account selection is persisted in the selected local profile. For
one operation, use the command's `--account` option or a wrapper's
`--rvs-account` option. `rvs context current` verifies the user and shows the
effective profile, account, target, and selection provenance.

## Artifact target

```bash
rvs art current
rvs art select platform/backend
rvs art clear
```

Target forms are `namespace/repository`, `mirror:source`, and
`custom-mirror:name`. Prefer one-shot `--target` or wrapper `--rvs-target` when
the user did not ask to change saved selection.

Resolution precedence is:

1. Explicit one-shot target.
2. Selected target for the effective local profile and acting account.
3. Legacy per-format private-repository default.
4. For supported reads, the account's enabled official cache.

An explicit account is resolved before the target. A failed authorization or
missing immutable target must not trigger a retry against another account or a
same-named repository.
