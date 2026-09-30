# Privacy Policy — IT-Tools

> Review items covered: C3 (policy provided) · C4 (completeness) · C5 (accessibility) ·
> C6 (data collection consistency) · C7 (user rights) · C8 (third-party disclosure)
>
> Public URL: <https://github.com/RyanYang163/shh1-it-tools/blob/main/PRIVACY.md>

**Effective date**: 2026-09-30
**Applies to**: 1.0.010
**Publisher**: shh (TOS 7 platform integration package)
**Package**: `shh1-it-tools` (Deb)
**Upstream project**: IT-Tools — <https://github.com/CorentinTh/it-tools> (author: Corentin Thomasset)

> This repository is a **TOS 7 platform integration package**, not a fork of the upstream project.
> The upstream is a pure front-end application with no data-collection logic of its own; everything
> below covers only what this package itself does on the device.

---

## 1. Data Collected

IT-Tools is a browser-only toolbox: every tool (JSON formatting, Base64, UUID, hashing, timestamps,
regular expressions, …) runs **entirely in the visitor's browser**. Whatever is typed into a tool is
computed locally and is never sent anywhere.

What this package itself handles on the device:

- **Static files** served to the browser from `/usr/local/shh1-it-tools/webui/` — read-only.
- **Access logs** written by the bundled local web server, held in the system journal: source IP,
  timestamp, requested path, user agent. They never leave the device.

**The publisher collects nothing.** No analytics, no telemetry, no crash reporting; no account with
the publisher is required or even possible.

## 2. Retention Period

- **No tool input and no tool result is ever stored.** Every calculation is discarded when the page
  is closed or reloaded.
- The only runtime record is the **access log**. It is held by `systemd-journald` on the device and
  is rotated and expired by the system's own journal policy. There is no developer-side retention
  window, because the publisher operates no server.
- Nothing else is written under `/var/lib/shh1-it-tools` or `/var/log/shh1-it-tools`; those
  directories are created for the service account but hold no user content.

## 3. Third-Party Sharing and Data Location

- **No third party is involved and no data leaves the device.**
- The application makes **no outbound network request of its own**. All static assets are served
  from inside the package — no CDN, no external font, no external script.
- The packaged service listens on the loopback interface only (`127.0.0.1:18801`); the external
  entry point is provided by TOS, and this package exposes no additional port.

| Feature | Destination | Default |
|---|---|---|
| Tool computations | browser only | enabled |
| Telemetry | none configured | **absent** |
| Outbound requests | none | **none** |

- **Data location**: the device the user installed the application on. The publisher stores no copy
  anywhere, in any region.

## 4. Security Measures

This is a Deb application. The statements below describe what is actually configured in
`init.d/shh1-it-tools.service` and can be verified there line by line.

- Runs as a dedicated **non-root** account with a disabled login shell (`User=shh1ittools`).
- systemd hardening — every item below is actually enabled in the unit:
  `NoNewPrivileges` · `ProtectSystem=strict` · `ProtectHome` · `PrivateTmp` · `PrivateDevices` ·
  `ProtectKernelTunables` · `ProtectKernelModules` · `ProtectControlGroups` · `RestrictRealtime` ·
  `RestrictSUIDSGID` · `LockPersonality` · `RemoveIPC` · `SystemCallArchitectures=native`
- Writable paths are limited to the package directory and `/var/lib/shh1-it-tools`
  (`ReadWritePaths=`); the rest of the file system is read-only to the service.
- The install / remove scripts perform **no network operation** — no `apt`, no `pip`, no `curl`.
- No credential or secret is shipped in the package, and no data passes through a
  publisher-operated server.

## 5. User Rights

There is no account system and no user dataset. The administrator of the device can, at any time and
without contacting the publisher:

- **Access**: read the access log with `journalctl -u shh1-it-tools`.
- **Erase**: delete that log — see section 6 — after which no trace of past visits remains on the
  device.
- **Opt out**: no usage data is collected in the first place, and the application has no telemetry to
  switch off.

## 6. Deletion Channel

1. **Logs**: `journalctl --vacuum-time=1s -u shh1-it-tools`, or let the system journal policy rotate
   them out.
2. **Uninstall with data**: uninstall in the TOS App Center and select "delete data" (equivalent to
   `dpkg --purge shh1-it-tools`) — this removes `/var/lib/shh1-it-tools`,
   `/var/log/shh1-it-tools` and `/usr/local/shh1-it-tools` entirely.
3. **Manual**: delete those three directories on the device.

Uninstalling **without** selecting "delete data" keeps `/var/lib/shh1-it-tools` and
`/var/log/shh1-it-tools`, so the app can be reinstalled without losing its service account state.
This is deliberate. There is no publisher-side copy to delete.

## 7. Contact

- **Packaging** — installation, uninstallation and TOS-integration questions:
  <https://github.com/RyanYang163/shh1-it-tools/issues>
- **Upstream project** — questions about IT-Tools' features themselves:
  <https://github.com/CorentinTh/it-tools/issues>

Questions about this policy can be raised in either tracker. Note that the upstream tracker is
maintained by the upstream author, who is not responsible for the installation or removal of this
TOS package.
