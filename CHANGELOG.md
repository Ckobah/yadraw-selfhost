# Changelog

## 0.1.2 — first published public release

First completed public self-hosted release.

Historical note: the earlier `0.1.0` and `0.1.1` release attempts were stopped
by the release security gate before any public distribution and were never
published. `0.1.2` is the first publicly released version.

### Included

- Local profile for Windows 10/11 x64, Linux x64 and Linux arm64
- Server profile for Linux with Caddy, automatic HTTPS and systemd
- Local authentication with first-administrator setup
- Encrypted backup and restore (PostgreSQL + MinIO + configuration)
- Staged, verified updates
- Signed releases with checksums, SBOMs and provenance
- Corresponding Source archive (`yadraw-source-<version>.tar.gz`) under
  AGPL-3.0-only, published with each release
- Personal-data scanner hardening: ISO-8601 release metadata timestamps are
  no longer misclassified as phone numbers
- Personal-data scanner context classification: phone-like digit runs inside
  machine tokens (SHA-256 hashes, image digests, signing key IDs, UUIDs,
  ISO-8601 timestamps) are no longer misclassified as phone numbers, while
  realistic phone numbers in text and JSON remain blocked

### Known limitations

- Single-machine installations (no cluster mode)
- No scheduled or remote backup destinations
- No automatic updater service

## 0.1.1 — release attempt (not published)

The `0.1.1` release attempt was stopped by the release security gate before
public distribution. No public artifacts of `0.1.1` were published.

## 0.1.0 — initial release attempt (not published)

The `0.1.0` release attempt was stopped by the release security gate before
public distribution. No public artifacts of `0.1.0` were published.
