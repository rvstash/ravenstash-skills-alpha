# ORAS workflows

Load when the user explicitly wants ORAS or an OCI-native discover, manifest,
tag, pull, or push operation.

ORAS supports both Ravenstash OCI kinds, so always provide one:

```bash
rvs oras \
  --rvs-kind container \
  --rvs-target platform/runtime-images \
  discover oci.rvsta.sh/in_abcdefgh/r_23456789/api:latest
```

Use `--rvs-kind helm` only for an exact Helm lane. Do not infer kind from the
shared host alone.

For a push, inspect the local artifact set and annotations, resolve the stable
reference with `rvs oci-reference`, and confirm the exact remote reference
before executing. Do not use ORAS to work around a missing repository lane or
to target a private mirror.
