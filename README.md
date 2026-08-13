# Yadraw Self-Hosted

Yadraw Self-Hosted is a self-contained runtime for running your own Yadraw
instance. It bundles everything you need on one machine: PostgreSQL, MinIO,
the Yadraw API and web UI, and (for the server profile) a Caddy reverse proxy
with automatic HTTPS.

## Platforms

- Windows 10/11 x64
- Linux x64
- Linux arm64

## Quick start

1. Install Docker Engine with the Compose v2 plugin (Docker Desktop on
   Windows).
2. Download the installer bundle for your platform from the
   [releases](https://github.com/Ckobah/yadraw-selfhost/releases) page and
   verify it against the published checksums and signature.
3. Run the installer:
   - Windows: `Install Yadraw.ps1`
   - Linux: `./install.sh`
4. Open `http://127.0.0.1:3000` (local profile) and complete `/setup` to
   create the first administrator account.

For a public server with HTTPS, see
[SELF_HOSTING.md](SELF_HOSTING.md#server-profile).

## Documentation

- [SELF_HOSTING.md](SELF_HOSTING.md) — install, server profile, HTTPS,
  systemd, lifecycle
- [ARCHITECTURE.md](ARCHITECTURE.md) — services and data layout
- [SECURITY.md](SECURITY.md) — authentication, encryption, update trust
- [BACKUP_AND_RESTORE.md](BACKUP_AND_RESTORE.md) — backups and restores
- [UPGRADE_POLICY.md](UPGRADE_POLICY.md) — how updates work
- [CHANGELOG.md](CHANGELOG.md) — release history
