---
name: ravenstash-oci
description: Use Ravenstash private Container and Helm OCI repository lanes through rvs, Docker, Helm, and ORAS with exact target and registry-kind selection and ephemeral credentials. Use for OCI references, image push or pull, Helm chart publish or install, and ORAS discovery; do not use for PyPI, npm, Maven, or private-mirror workflows.
license: MIT
metadata:
  author: Ravenstash
  version: "0.4.0"
  ravenstash-rvs-compatibility: "0.9.3 or later in the 0.9 channel"
---

# Ravenstash OCI

Container and Helm are distinct private repository kinds that share OCI
Distribution and the `oci.rvsta.sh` host. Never collapse them into one implicit
lane.

## Preflight

1. Run `rvs --version` and discover current syntax with `rvs <group> --help`.
2. Check `rvs auth status` and use `rvs auth whoami` when identity matters.
3. Resolve the acting account and exact `namespace/repository` target.
4. Resolve `container` or `helm`. ORAS always needs an explicit kind.
5. Confirm that the requested native tool is installed.

Read [references/container.md](references/container.md) for Docker image work,
[references/helm.md](references/helm.md) for Helm charts, or
[references/oras.md](references/oras.md) for ORAS. Load only the relevant file.

## Stable reference construction

Use `rvs oci-reference` rather than inventing namespace or repository refs:

```bash
rvs oci-reference \
  --kind container \
  --target platform/runtime-images \
  --oci-path api \
  --reference latest
```

The returned reference contains stable Ravenstash namespace and repository
identifiers. Never reconstruct those identifiers from display names.

## Credential boundary

- Use the `rvs docker`, `rvs helm`, or `rvs oras` wrapper for temporary scoped
  authentication.
- Do not extract a token for `docker login`, put it in argv, or write it to the
  user's Docker or Helm configuration.
- The wrapper uses a temporary native configuration while preserving unrelated
  registry credentials. Do not replace the user's complete native config.
- Inspect verbose native logs before sharing them.

## Target boundary

- OCI targets are exact private repositories. `mirror:` and `custom-mirror:`
  targets are invalid.
- A one-shot `--rvs-target` must not change saved selection.
- Pull, inspect, show, and discover operations are normally reads. Push and
  publish operations create remote state and require the exact destination to
  be confirmed.
- Do not invent a tag, chart version, digest, or OCI path that the user did not
  supply or that cannot be read from the project.

## Completion

Report registry kind, target, stable OCI reference, native tool, and observable
result. For a push or publish, include the resulting tag or digest when the
native tool reports it, without exposing credentials.
