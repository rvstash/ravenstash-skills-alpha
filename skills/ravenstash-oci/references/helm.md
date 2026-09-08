# Helm OCI workflows

Load for Helm chart publication, inspection, pull, or install against a
Ravenstash Helm lane.

## Resolve the reference

```bash
rvs oci-reference \
  --kind helm \
  --target platform/deployment-charts \
  --oci-path charts/api \
  --reference 1.2.3
```

Use an `oci://` reference where Helm requires it. Keep the chart name and
version separate from the Ravenstash repository display name.

## Read

```bash
rvs helm --rvs-target platform/deployment-charts show chart \
  oci://oci.rvsta.sh/in_abcdefgh/r_3456789a/charts/api \
  --version 1.2.3
```

## Publish

A Helm push creates remote state. Confirm the packaged chart, metadata version,
exact target, and OCI path before invoking the native command through `rvs
helm`. Do not publish to a Container lane merely because it shares the same
host.

Remote caches are not supported for Helm repositories.
