# IT-Tools

> TOS 7 application package for **IT-Tools** — platform integration only.
> The application itself is provided by the upstream project, unmodified.

## Overview

A collection of handy online tools for developers: JSON formatting, Base64, UUID, hashes, timestamps and more.

上游项目 / Upstream: <https://github.com/CorentinTh/it-tools>
上游许可证 / License: **GPL-3.0**

## Features

- 80+ developer utilities in one page
- Runs fully in the browser; no server-side processing
- No database, no user accounts, no data collection

## Installation

1. Requirements: TOS 7.0+ and systemd + nginx
2. Install from the TOS App Center
3. Open the app and complete initial configuration

## Usage

1. Access URL: `/shh1-it-tools/`
2. Default credentials: see upstream documentation
3. Key settings: see upstream documentation

## Permissions

| Permission | Rationale |
|---|---|
| Network: port 18801 | Web UI access |
| File system: `/Volume*/DockerAppData/shh1-it-tools/` | Application data persistence |
| User: shh1ittools | Isolated non-root service execution |

## Configuration

See `config.ini` for platform metadata; see `docker-compose.yml` for runtime configuration.

## Ports

| Port | Protocol | Purpose |
|---|---|---|
| 18801 | TCP | Web UI (IT-Tools) |

## Support

- Documentation: https://github.com/CorentinTh/it-tools
- Issue tracker: https://github.com/CorentinTh/it-tools/issues
- Community: https://github.com/CorentinTh/it-tools

## Security & Compliance

- **License**: GPL-3.0 — full text in [`LICENSE`](./LICENSE)
- **Attribution**: see [`NOTICE`](./NOTICE)
- **Privacy Policy**: see [`PRIVACY.md`](./PRIVACY.md)
- **Vulnerability scan**: `trivy-report.txt` attached to each Release (HIGH/CRITICAL must be 0)
- Runs as a non-root dedicated user; no privileged mode, no host network

## Changelog

### v1.0.1 (2026-09-20)
- Compliance update: added LICENSE / NOTICE / PRIVACY materials,
  declared upstream license inside the package, added container healthcheck

### v1.0.0
- Initial release

## License

**GPL-3.0** — this packaging repository is distributed under the same license as the
upstream project. Full text: [`LICENSE`](./LICENSE).
