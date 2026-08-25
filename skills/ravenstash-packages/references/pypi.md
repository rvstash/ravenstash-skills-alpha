# PyPI workflows

Load for pip, uv, Twine, Python dependency, wheel, or source-distribution work.

## Install

Use a private repository once:

```bash
rvs pip --rvs-target platform/packages install internal-sdk
rvs uv --rvs-target platform/packages sync
```

Use a direct official cache once:

```bash
rvs pip --rvs-target cache:pypiorg install requests
```

Ravenstash must be the primary index for a selected Ravenstash resolution
workflow. Do not recommend pip `--extra-index-url`; dependency-confusion rules
can allow the unintended index to win. Use a repository upstream attachment or
an explicit direct-cache target instead.

`rvs pkg pypi install` is also valid for a Ravenstash-owned install helper. Use
the native wrapper when preserving the caller's pip or uv command is the goal.

## Publish

Publishing requires a private PyPI repository, never a cache:

```bash
rvs twine --rvs-target platform/releases upload dist/*
rvs pkg pypi publish dist/ --repo platform/releases
```

Inspect the distribution files and exact repository before publishing. A
publish creates durable remote state and is not a preparatory step that may be
invented to satisfy another request.

## Persistent configuration

`rvs pkg pypi index-url` and `upload-url` are read operations. A configure
command or editing pip/Twine configuration changes persistent local state and
requires explicit intent. Never write a Ravenstash token into that state.
