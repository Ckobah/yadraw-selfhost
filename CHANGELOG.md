# Changelog

## 0.1.5 — first published public release (recovery candidate)

First completed public self-hosted release.

Historical note: the earlier `0.1.0`–`0.1.4` release attempts were stopped by
the release security gate before a clean public installation was possible.
`0.1.5` is the first completed public release.

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
- Phone PII detection now uses libphonenumber-js validation with context-aware
  classification (`phone-pii-scanner.mjs`): arbitrary digit sequences without
  phone context are not personal data, and machine tokens are never phones
- Release pipeline hardening: the public distribution tag is created only
  after all release assets are verified, and the tag step is safe to re-enter
  when the existing tag already points to the exact public distribution commit
- Release jobs that execute dependency-requiring scripts now install the
  committed lockfile (`npm ci`) in their own isolated environments, matching
  the dependency graph verified by the quality job
- Self-Hosted runtime images moved to dedicated public GHCR packages
  (`ghcr.io/ckobah/yadraw-selfhost-{web,api,init}`); clean installations no
  longer require access to the private SaaS/development image packages
- Release pipeline verifies anonymous GHCR pull before any public
  distribution or GitHub Release mutation

### Known limitations

- Single-machine installations (no cluster mode)
- No scheduled or remote backup destinations
- No automatic updater service

## 0.1.4 — release attempt (not published for clean install)

The `0.1.4` pipeline and public release completed, but clean installation
acceptance was blocked because the referenced GHCR packages required
authentication. `0.1.5` moves the Self-Hosted runtime images to dedicated
public GHCR packages.

## 0.1.3 — release attempt (not published)

The `0.1.3` release attempt created the source tag, but the release pipeline
stopped fail-closed at the preflight readiness gate: the isolated release job
had no installed npm dependencies for the new phone PII scanner
(libphonenumber-js). No `0.1.3` GHCR images, public distribution, tag or
GitHub Release were published.

## 0.1.2 — release attempt (not published)

The `0.1.2` release attempt created the public distribution and the public
`v0.1.2` tag, but stopped fail-closed in the release publishing job before the
GitHub Release and its assets were published. No `0.1.2` GitHub Release or
public assets exist.

## 0.1.1 — release attempt (not published)

The `0.1.1` release attempt was stopped by the release security gate before
public distribution. No public artifacts of `0.1.1` were published.

## 0.1.0 — initial release attempt (not published)

The `0.1.0` release attempt was stopped by the release security gate before
public distribution. No public artifacts of `0.1.0` were published.
