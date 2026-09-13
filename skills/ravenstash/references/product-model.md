# Ravenstash product model

Load this reference only when the request depends on Ravenstash terminology,
target selection, registry support, or availability.

## Ownership and content

- A user is one person with a Ravenstash login.
- A local profile is named CLI configuration and credential association; it is
  not the authenticated user.
- An acting account is the personal account or organization authorizing the operation
  and receiving its usage attribution.
- A namespace is an account-owned cross-product naming and organization-member
  permission boundary.
- A repository is a private logical content namespace that can enable one or
  more registry kinds.
- A repository lane is one registry kind inside a repository. PyPI, npm, Maven,
  Container, and Helm are separate lanes even when they share a repository.
- A private mirror is an account-scoped, read-only PyPI, npm, or Maven access
  surface backed by an official or custom remote cache. It is not a private
  repository and cannot accept publishing.

## Target forms

| Form | Meaning | Capabilities |
| --- | --- | --- |
| `namespace/repository` | Private repository | Read and publish for enabled lanes |
| `mirror:source` | Ravenstash-curated official private mirror | Read only |
| `custom-mirror:name` | Account-defined custom private mirror | Read only |

An explicit target is one-shot and must not change saved selection. A saved
target remains scoped to its local profile and immutable acting-account identity.

## Registry boundaries

- PyPI, npm, and Maven support private repositories and private mirrors.
- Container and Helm are separate private-only OCI registry kinds.
- Container and Helm share the `oci.rvsta.sh` host but never become one lane.
- ORAS can address either OCI kind, so the kind must be explicit.
- A mirror's backing remote cache can be connected behind a compatible private
  repository. The
  attachment copies its age setting when created and is then managed
  independently from the mirror default.

## Availability floor for these skills

These sources prepare the unreleased Task 084 command surface.
`rvs art repo` manages repositories for packages, container images, and Helm charts.
Top-level `rvs repo`, `rvs artifacts`, and `rvs ci` are absent. Check current public documentation before
making broader product-availability claims.
