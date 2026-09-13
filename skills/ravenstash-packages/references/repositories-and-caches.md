# Repositories and private mirrors

Load for repository discovery or mutation, package lifecycle work, private-mirror
management, or upstream attachment.

## Private repositories

Inspect before changing:

```bash
rvs art repo list
rvs art repo list --format pypi
rvs art repo show platform/packages
```

Creation requires an explicit name and one or more registry kinds:

```bash
rvs art repo create packages --format pypi,npm
rvs art repo create packages --format pypi --format npm
```

A logical repository can contain several lanes. Confirm all intended kinds
rather than creating another same-named repository to compensate for a missing
lane.

Repository mutations include:

```bash
rvs art repo rename platform/packages new-name
rvs art repo set-default pypi platform/packages
rvs art repo delete platform/packages
```

For deletion, keep the CLI confirmation unless the exact resolved deletion was
already authorized. Do not infer retention or recovery guarantees; point to
current public documentation.

## Private mirrors

Private mirrors are read-only and support only PyPI, npm, and Maven. Each is
backed by an official or customer-defined remote cache, and their targets remain
distinct:

```bash
rvs art mirror list
rvs art mirror create pypiorg --select
rvs art mirror select pypiorg
rvs art mirror select --custom company-python
```

Use a mirror once without changing the saved target:

```bash
rvs art install requests --target mirror:pypiorg
rvs art install internal-sdk --target custom-mirror:company-python
```

Create custom mirrors in the webapp. The CLI can select, read, manage, and attach
existing custom mirrors. It cannot create them.

## Attach a backing remote cache to a repository

```bash
rvs art mirror show CACHE_ID
rvs art repo upstream add platform/packages pypi \
  --remote-cache CACHE_ID \
  --min-age-hours 24
```

The attachment copies the cache's age setting at creation and is then managed
independently. A later mirror default change must not be described as
automatically changing existing attachments.

## Package lifecycle

Resolve repository and kind before acting:

```bash
rvs art package list --target platform/packages --format pypi
rvs art package show requests --target platform/packages --format pypi
```

Delete and yank operations affect remote package state. Confirm exact package,
version, repository, registry kind, and account before invoking them. Do not add
`--yes` unless the exact deletion is already authorized.
