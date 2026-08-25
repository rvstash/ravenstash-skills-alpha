# npm workflows

Load for npm, Node.js, JavaScript, TypeScript, scoped-package, install, or
publish work.

## Install

```bash
rvs npm --rvs-target platform/packages install @acme/design-system
rvs npm --rvs-target platform/packages ci
rvs npm --rvs-target cache:npmjs install lodash
```

Direct caches support reads only. Keep Ravenstash wrapper options before the
native npm arguments. A one-shot target must not change saved selection.

## Publish

Publishing requires a private npm repository:

```bash
rvs npm --rvs-target platform/releases publish
rvs pkg npm publish . --repo platform/releases
```

The Ravenstash-native publish helper uses native `npm pack`, including the
package's packlist and lifecycle hooks. Treat those hooks as project code that
may execute locally; inspect the package and obtain any execution approval
required by the environment before publishing.

## Configuration

`rvs pkg npm registry-url` and `npmrc` can render non-secret routing material.
Persistent configuration changes require explicit intent. Do not place an
access token in a tracked `.npmrc`, command argument, or output.
