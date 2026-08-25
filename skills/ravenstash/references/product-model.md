# Ravenstash product model

Load this reference only when the request depends on Ravenstash terminology,
target selection, registry support, or availability.

## Ownership and content

- A user is one person with a Ravenstash login.
- An account is the personal account or organization authorizing the operation
  and receiving its usage attribution.
- A workspace is an account-owned repository namespace and organization-member
  permission boundary.
- A repository is a private logical content namespace that can enable one or
  more registry kinds.
- A repository lane is one registry kind inside a repository. PyPI, npm, Maven,
  Container, and Helm are separate lanes even when they share a repository.
- A remote cache is an account-scoped, read-only PyPI, npm, or Maven source. It
  is not a private repository and cannot accept publishing.

## Target forms

| Form | Meaning | Capabilities |
| --- | --- | --- |
| `workspace/repository` | Private repository | Read and publish for enabled lanes |
| `cache:source` | Ravenstash-curated official cache | Read only |
| `custom-cache:name` | Account-defined custom cache | Read only |

An explicit target is one-shot and must not change saved selection. A saved
target remains scoped to its login profile and immutable account identity.

## Registry boundaries

- PyPI, npm, and Maven support private repositories and direct remote caches.
- Container and Helm are separate private-only OCI registry kinds.
- Container and Helm share the `oci.rvsta.sh` host but never become one lane.
- ORAS can address either OCI kind, so the kind must be explicit.
- A cache can be connected behind a compatible private repository. The
  attachment copies its age setting when created and is then managed
  independently from the direct cache default.

## Availability floor for these skills

These skills cover Ravenstash Packages and the `rvs` 0.4 compatibility channel.
They do not make the placeholder `rvs repo` or `rvs ci` groups executable and
do not describe future public repositories as available. Check current public
documentation before making broader product-availability claims.
