# Self-hosting Yadraw

## Requirements

- Docker Engine with the Compose v2 plugin (Docker Desktop on Windows)
- Local profile: no root privileges
- Server profile (Linux): root privileges, public DNS pointing at the host,
  inbound TCP 80 and 443

## Install

Download the installer bundle for your platform from the
[releases page](https://github.com/Ckobah/yadraw-selfhost/releases) and run:

- Windows: `Install Yadraw.ps1`
- Linux: `./install.sh`

After installation the web UI is available at `http://127.0.0.1:3000` (or the
port you chose). Open `/setup` in the browser to create the first
administrator account.

## Server profile

On Linux, install the server profile to get HTTPS, a Caddy reverse proxy and
automatic startup:

```text
sudo ./install.sh \
  --profile=server \
  --domain=yadraw.example.com \
  --email=admin@example.com \
  --data-dir=/var/lib/yadraw \
  --yes
```

Requirements:

- the domain resolves to this server (A or AAAA record);
- TCP 80 and 443 are free and reachable from the internet;
- the server runs as root so the systemd service can be installed.

The installation requests a TLS certificate automatically (Let's Encrypt).
After a successful install, `https://<domain>/setup` opens the initial
administrator setup, and HTTP requests redirect to HTTPS.

The server profile installs and enables a systemd unit
(`yadraw-selfhost-<project>.service`) so the stack starts automatically after
a reboot.

## Lifecycle

The bundle ships launchers for the installed instance. Run them from the
extracted bundle directory:

Linux:

```text
./launchers/linux/start.sh
./launchers/linux/stop.sh
./launchers/linux/restart.sh
./launchers/linux/status.sh
./launchers/linux/doctor.sh
./launchers/linux/logs.sh [service]
./launchers/linux/uninstall.sh
```

Windows (PowerShell):

```powershell
.\launchers\Start Yadraw.ps1
.\launchers\Stop Yadraw.ps1
.\launchers\Restart Yadraw.ps1
.\launchers\Status Yadraw.ps1
.\launchers\Doctor Yadraw.ps1
.\launchers\Logs Yadraw.ps1 [service]
.\launchers\Uninstall Yadraw.ps1
```

`doctor` checks the environment and installation state. `uninstall` keeps
your data unless you pass `--delete-data --yes`.

## Data directory

Installations keep state in a data directory (default platform app data
directory, or the one passed with `--data-dir`):

- `.env` — runtime configuration and secrets (generated at install);
- `assets/` — frozen compose/Caddy files used by the lifecycle;
- `installation-manifest.json` — installed version and image metadata.

PostgreSQL and MinIO data live in Docker volumes; Caddy certificates live in
the Caddy data volume (server profile).

## Uninstall

Keep-data uninstall stops the stack and disables the systemd unit while
preserving the data directory and volumes. Destructive uninstall
(`--delete-data --yes`) removes containers, volumes, the systemd unit and the
data directory.
