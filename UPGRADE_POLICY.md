# Upgrade policy

## How updates work

Run the update launcher from the extracted bundle directory:

Linux:

```text
./launchers/linux/update.sh --check
./launchers/linux/update.sh --version=<v> --yes
./launchers/linux/update.sh --release-dir=<path> --yes
```

Windows (PowerShell):

```powershell
.\launchers\Update Yadraw.ps1 --check
.\launchers\Update Yadraw.ps1 --version=<v> --yes
.\launchers\Update Yadraw.ps1 --release-dir=<path> --yes
```

`--check` only queries the latest release and changes nothing. Applying an
update (including a local staged release) requires `--yes`.

Updates always create a verified safety backup first. The new version is
started only after the candidate stack passes its health check, and the
installation manifest is committed only after success.

## Trust

- Published releases are signed; the updater verifies the signature and
  content hashes before applying anything.
- Discovery metadata (`releases/latest.json`) is only an index and is never
  trusted by itself.
- Signed releases are mandatory for network updates; unsigned staged releases
  require an explicit local opt-in flag.

## Compatibility

- Releases are `expand-compatible`: they may add features but never perform
  destructive schema changes without a separate migration release.
- External images (PostgreSQL, MinIO, Caddy) are pinned; a release cannot
  silently change them.
- Downgrades are refused.

## Artifacts

Installer bundles for Windows x64, Linux x64 and Linux arm64 are published to
the
[releases page](https://github.com/Ckobah/yadraw-selfhost/releases) together
with checksums, signatures, SBOMs and provenance.
