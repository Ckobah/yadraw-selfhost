# Backup and restore

## Backup

Run the backup launcher from the extracted bundle directory:

Linux:

```text
./launchers/linux/backup.sh --data-dir=<path> --output-dir=<path> --passphrase-file=<path>
```

Windows (PowerShell):

```powershell
.\launchers\Backup Yadraw.ps1 --data-dir=<path> --output-dir=<path> --passphrase-file=<path>
```

- Produces `yadraw-backup-<timestamp>.tar.gz` (UTC) in `--output-dir`
  (default: the current directory; never inside the data directory).
- The archive contains a PostgreSQL dump (`pg_dump` custom format), MinIO
  object data, an encrypted configuration envelope, manifests and checksums.
- Backup briefly pauses the write path (web/API and object storage) so the
  snapshot is consistent, then restores the stack and verifies health.
- The passphrase is required to restore. It is never accepted as a command
  line value; use the interactive prompt or `--passphrase-file`. Minimum
  length is 12 characters. There is no way to recover a lost passphrase.

## Restore

Linux:

```text
./launchers/linux/restore.sh <backup-file> \
  --data-dir=<path> --passphrase-file=<path> --yes
```

Windows (PowerShell):

```powershell
.\launchers\Restore Yadraw.ps1 <backup-file> `
  --data-dir=<path> --passphrase-file=<path> --yes
```

- Restore replaces PostgreSQL and MinIO with the backup state.
- The backup is fully validated (structure, HMAC, checksums, decryption,
  dump integrity) before anything destructive happens.
- Restoring into an existing installation requires the same profile and
  project, and creates a `yadraw-pre-restore-*` safety backup first.
- Restoring into a fresh data directory rebuilds the installation manifest
  for the target machine (fresh platform/architecture, real image IDs).
- Version policy: a backup can be restored with the same release version it
  was created from. Cross-version moves use the update path.

## Encryption

- Key derivation: scrypt.
- Encryption: AES-256-GCM with a fresh random salt and IV per backup.
- Authentication: HMAC-SHA256 over the manifest and checksum set.

Wrong passphrase or a tampered backup is rejected before any data is touched.
