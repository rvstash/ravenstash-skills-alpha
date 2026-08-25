# Installation, updates, runtimes, and shell integration

Load only for installation, upgrades, runtime management, or shell changes.

## Install and update

The supported end-user install path for Ubuntu 20.04+ and Debian 11+ on Linux
`amd64` is:

```bash
curl -fsSL https://ravenstash.com/install.sh | bash
```

This executes downloaded code and configures APT. Show the command and point to
<https://ravenstash.com/install.sh> for review; run it only when the user has
authorized the host-level installation.

Check for compatible updates without applying one:

```bash
rvs update
```

Applying an update changes the installed system package:

```bash
rvs update --apply
```

Before 1.0, each minor line is a compatibility boundary. Crossing it is
explicit and should follow release-note review:

```bash
rvs upgrade --to 0.4
```

Do not replace the binary manually or bypass the signed APT path.

## Managed runtimes

Runtime management is optional and supports local Python, Node.js, and Java:

```bash
rvs runtime install python 3.14
rvs runtime install node 22
rvs runtime install java 21
rvs runtime list
rvs runtime doctor
```

Installation downloads content and changes `~/.rvs/runtimes`. `--force` removes
and replaces an existing matching runtime; do not use it unless requested.

Project selection writes a conventional local file:

```bash
rvs runtime use python 3.14
rvs runtime use node 22
rvs runtime use java 21
```

Report `.python-version`, `.node-version`, or `.java-version` as a changed file
and do not assume it should be committed.

## Shell changes

- `rvs runtime setup-shell` installs runtime shims.
- `rvs shell setup` adds visible profile, account, and target context.

Both can edit a shell startup file. Inspect or report the change and do not
apply it merely because a single command needs environment variables.
