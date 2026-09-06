# Container workflows

Load for Docker or OCI image operations against a Ravenstash Container lane.

## Resolve the reference

```bash
rvs oci-reference \
  --kind container \
  --target platform/runtime-images \
  --oci-path api \
  --reference latest
```

Use the exact returned reference in subsequent commands. Do not derive stable
namespace or repository identifiers from display names.

## Read and write

Pull through the wrapper:

```bash
rvs docker --rvs-target platform/runtime-images pull \
  oci.rvsta.sh/w_abcdefgh/r_23456789/api:latest
```

A push creates remote state. Before pushing, confirm the local image, exact
remote path and tag, acting account, and Container target. Use the wrapper and
the native Docker push syntax; never perform a separate persistent login as a
preparatory step.

Remote caches are not supported for Container repositories.
