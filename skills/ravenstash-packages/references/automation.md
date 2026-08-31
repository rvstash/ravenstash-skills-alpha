# Package automation

Load for CI, release jobs, non-interactive installs, or publishing.

## Authentication

Inject an eligible automation token from the CI system's secret store:

```bash
export RVS_TOKEN="${TOKEN_FROM_SECRET_STORE}"
rvs auth whoami
```

Do not run device login in CI. `RVS_TOKEN` is not refreshed, and its ownership
determines the available accounts and repositories.

## Make context explicit

Prefer one-shot profile-independent account and target selection in shared
runners:

```bash
rvs npm \
  --rvs-account org:acme \
  --rvs-target platform/packages \
  ci
```

Use equivalent wrappers for pip, uv, Twine, or Maven. A job should not depend on
an interactive user's selected account or target.

## Logging and cleanup

- Do not enable shell tracing around token export or authenticated invocations.
- Review verbose native-client logs before publishing them as artifacts.
- Do not write the token to a generated package-manager configuration file.
- Remove temporary files created by the job through its normal workspace
  cleanup; do not delete user configuration outside the runner workspace.

`rvs ci` is a placeholder in the 0.7 channel. CI workflows use the package
wrappers or documented native package-manager configuration instead.
