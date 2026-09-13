# npm workflows

Load for npm, Node.js, JavaScript, TypeScript, scoped-package, install, or
publish work.

## Install

```bash
rvs npm --rvs-target platform/packages install @acme/design-system
rvs npm --rvs-target platform/packages ci
rvs npm --rvs-target mirror:npmjs install lodash
```

Private mirrors support reads only. Keep Ravenstash wrapper options before the
native npm arguments. A one-shot target must not change saved selection.

## Publish

Publishing requires a private npm repository:

```bash
rvs npm --rvs-target platform/releases publish
rvs art npm publish . --target platform/releases
```

The Ravenstash-native publish helper uses native `npm pack`, including the
package's packlist and lifecycle hooks. Treat those hooks as project code that
may execute locally; inspect the package and obtain any execution approval
required by the environment before publishing.

## Configuration

`rvs art endpoint --format npm` prints routing; `rvs art native config npm`
prints setup instructions without minting credentials or writing files.
Persistent configuration changes require explicit intent. Do not place an
access token in a tracked `.npmrc`, command argument, or output.
