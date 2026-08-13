# Security

## Local authentication

The first administrator is created through `/setup`. After that, login is
handled by Yadraw's local authentication: passwords are stored hashed,
sessions use server-side cookies, and login attempts are rate-limited.

## Secrets

- Secrets are generated at install time into `.env` (mode 0600 on Linux) and
  are never printed to logs.
- The internal API secret protects web-to-API calls; it is never exposed to
  browsers.
- Object storage credentials are kept server-side.

## Transport

- Local profile: the web UI listens on the loopback interface.
- Server profile: HTTPS is terminated by Caddy with certificates from Let's
  Encrypt; HTTP redirects to HTTPS; Caddy sends security headers
  (`X-Content-Type-Options`, `Referrer-Policy`, `X-Frame-Options`, HSTS).

## Backups

Backups are encrypted with AES-256-GCM using a key derived from your
passphrase (scrypt), and are authenticated with HMAC-SHA256. A backup without
the passphrase cannot be restored.

See [BACKUP_AND_RESTORE.md](BACKUP_AND_RESTORE.md).

## Updates

Published releases are signed, and the updater verifies the release signature
and content hashes before applying anything. Discovery metadata is only an
index; it is never trusted by itself.

See [UPGRADE_POLICY.md](UPGRADE_POLICY.md).

## Release artifacts and Corresponding Source verification

Every release publishes `SHA256SUMS-<version>.txt` covering all release
assets, including the AGPL-3.0 Corresponding Source archive
`yadraw-source-<version>.tar.gz`. The source archive checksum is additionally
published as `yadraw-source-<version>.tar.gz.sha256`, signed with the release
signing key as `yadraw-source-<version>.tar.gz.sig` (Ed25519). To verify:

1. Verify the signature over the `.sha256` file with the public key recorded
   in the release metadata (`keyId` in `releases/latest.json`).
2. Confirm the `.sha256` value matches the downloaded archive.
3. Confirm `SOURCE_MANIFEST.json` inside the archive records the exact
   release version and source commit.

## Maintenance

Mutating commands (backup, restore, update, install, uninstall, lifecycle)
respect a maintenance lock so operations cannot run concurrently and corrupt
state.
