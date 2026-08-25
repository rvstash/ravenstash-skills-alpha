# Repositories and remote caches

Load for repository discovery or mutation, package lifecycle work, remote-cache
management, or upstream attachment.

## Private repositories

Inspect before changing:

```bash
rvs pkg repo list
rvs pkg repo list --registry-kind pypi
rvs pkg repo show platform/packages
```

Creation requires an explicit name and one or more registry kinds:

```bash
rvs pkg repo create packages --registry-kind pypi
```

A logical repository can contain several lanes. Confirm all intended kinds
rather than creating another same-named repository to compensate for a missing
lane.

Repository mutations include:

```bash
rvs pkg repo rename platform/packages new-name
rvs pkg repo set-default pypi platform/packages
rvs pkg repo delete platform/packages
```

For deletion, keep the CLI confirmation unless the exact resolved deletion was
already authorized. Do not infer retention or recovery guarantees; point to
current public documentation.

## Remote caches

Remote caches are read-only and support only PyPI, npm, and Maven. Official and
custom targets remain distinct:

```bash
rvs pkg cache list
rvs pkg cache add pypiorg --select
rvs pkg cache select pypiorg
rvs pkg cache select --custom company-python
```

Use a cache once without changing the saved target:

```bash
rvs pkg --target cache:pypiorg install requests
rvs pkg --target custom-cache:company-python install internal-sdk
```

When creating a protected custom cache, pass the secret by environment-variable
name, never by value on the command line:

```bash
export UPSTREAM_TOKEN="${TOKEN_FROM_SECRET_STORE}"
rvs pkg cache create-custom company-python \
  --kind pypi \
  --api-url https://packages.example.com/simple/ \
  --auth-scheme bearer \
  --secret-env UPSTREAM_TOKEN \
  --allowed-host packages.example.com
```

Do not disclose or persist the environment value. Check current documentation
for plan or role eligibility rather than encoding it in the skill.

## Attach a cache behind a repository

```bash
rvs pkg cache show CACHE_ID
rvs pkg repo set-upstream platform/packages CACHE_ID --min-age-days 3
```

The attachment copies the cache's age setting at creation and is then managed
independently. A later direct-cache default change must not be described as
automatically changing existing attachments.

## Package lifecycle

Resolve repository and kind before acting:

```bash
rvs pkg package list --repo platform/packages --registry-kind pypi
rvs pkg package show requests --repo platform/packages --registry-kind pypi
```

Delete and yank operations affect remote package state. Confirm exact package,
version, repository, registry kind, and account before invoking them. Do not add
`--yes` unless the exact deletion is already authorized.
