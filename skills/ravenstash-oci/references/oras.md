# ORAS workflows

Load when the user explicitly wants ORAS or an OCI-native discover, manifest,
tag, pull, or push operation.

ORAS uses the selected repository’s OCI format:

```bash
rvs oras \
  --rvs-target platform/runtime-images \
  discover oci.rvsta.sh/in_abcdefgh/r_23456789/api:latest
```

Do not pass `--rvs-format`. Content type comes from the manifest, not the host
or a credential permission. Use `rvs art oci` for typed path/tag/digest management.

For a push, inspect the local artifact set and annotations, resolve the stable
reference with `rvs art reference`, and confirm the exact remote reference
before executing. Do not use ORAS to work around a missing repository lane or
to target a private mirror.
