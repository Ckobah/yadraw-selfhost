# Architecture

Yadraw Self-Hosted runs a small set of containers on an internal Docker
network:

| Service | Purpose |
| --- | --- |
| `postgres` | Primary data store (PostgreSQL with pgvector) |
| `minio` | Object storage for attachments and files |
| `init` | One-shot initialization: applies migrations, creates the bucket |
| `api` | Backend API |
| `web` | Web application (Next.js) |
| `caddy` | Reverse proxy with automatic HTTPS (server profile only) |

## Network and ports

- Local profile: only the web UI is published on the host
  (`127.0.0.1:<port>`), usually port 3000.
- Server profile: only Caddy is published on the host (TCP 80/443). The API,
  web, PostgreSQL and MinIO are never exposed directly to the host.

## Data layout

- PostgreSQL data: `postgres_data` volume
- MinIO objects: `minio_data` volume
- Caddy state and certificates: `caddy_data` and `caddy_config` volumes
- Configuration and secrets: the data directory `.env`

## Configuration

All configuration lives in the `.env` file generated at install time. Key
values include the database credentials, object storage credentials, the
internal API secret, the authentication mode (`local`) and, for the server
profile, the public domain and ACME contact email.

The frozen compose/Caddy files in `assets/` are used by every lifecycle
command, so an installation never depends on a repository checkout.
