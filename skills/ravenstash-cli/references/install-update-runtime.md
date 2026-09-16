# Installation, updates, runtimes, and shell integration

Load only for installation, upgrades, runtime management, or shell changes.

## Install and update

The shell installer selects the released native build on supported Linux and
macOS systems:

```bash
curl -fsSL https://ravenstash.com/install.sh | bash
```

Windows 11 uses PowerShell:

```powershell
irm https://ravenstash.com/install.ps1 | iex
```

The supported release targets Linux glibc and musl, macOS, and Windows on amd64
and arm64, plus versioned Nix flakes and matching Linux paths for WSL2. The
installer executes downloaded code. Show the applicable command and installer
URL for review; run it only when the user has authorized the host-level
installation.

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
rvs update --to SERIES
rvs update --to SERIES --apply
```

`SERIES` is the requested newer release series. The first command only previews;
only `--apply` installs. `--yes` requires `--apply`.

APT installations delegate to APT. Portable Linux, Alpine, macOS, and Windows
installations authenticate, stage, health-check, and activate the matching
bundle. Nix installations delegate replacement to Nix. Do not replace the
binary manually or bypass the signed update path.

Signed release candidates can be previewed and installed explicitly:

```bash
rvs update --candidate X.Y.ZrcN
rvs update --candidate X.Y.ZrcN --apply
```

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
- The former `rvs shell setup` context prompt is removed; do not invoke it.

Runtime shim setup can edit a shell startup file. Inspect or report the change and do not
apply it merely because a single command needs environment variables.
