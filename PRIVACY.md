# Privacy Policy — IT-Tools

> Review items covered / 适用审核项：C3 (policy provided) · C4 (completeness) · C5 (accessibility) ·
> C6 (data collection consistency) · C7 (user rights) · C8 (third-party disclosure)
>
> Public URL / 公网地址：<https://github.com/RyanYang163/shh1-it-tools/blob/main/PRIVACY.md>

**Effective date / 生效日期**：2026-09-30
**Applies to / 适用版本**：1.0.008
**Publisher / 发布方**：shh（TOS 7 平台集成封装）
**Package / 包名**：`shh1-it-tools` (Deb)
**Upstream / 上游项目**：IT-Tools — <https://github.com/CorentinTh/it-tools>（作者 Corentin Thomasset）

> 本仓库是 **TOS 7 平台集成封装**，不是上游项目的分支。上游为纯前端应用，本身不含任何数据收集逻辑；
> 下面描述的范围仅覆盖本封装在设备上运行所必需的部分。
>
> This repository is a **TOS 7 platform integration package**, not a fork of the upstream project.
> The upstream is a pure front-end application with no data-collection logic of its own; everything
> below covers only what this package itself does on the device.

---

## 1. Data Collected / 收集哪些数据

IT-Tools is a browser-only toolbox: every tool (JSON formatting, Base64, UUID, hashing, timestamps,
regular expressions, …) runs **entirely in the visitor's browser**. Whatever is typed into a tool is
computed locally and is never sent anywhere.

IT-Tools 是纯浏览器工具箱：所有工具（JSON 格式化、Base64、UUID、哈希、时间戳、正则等）**完全在访问者的浏览器内运行**。
输入的内容在本地完成计算，不会被发送到任何地方。

What this package itself handles on the device / 本封装在设备上实际经手的内容：

- **Static files** served to the browser from `/usr/local/shh1-it-tools/webui/` — read-only.
  向浏览器提供的**静态文件**（`/usr/local/shh1-it-tools/webui/`），只读。
- **Access logs** written by the bundled local web server, held in the system journal: source IP,
  timestamp, requested path, user agent. They never leave the device.
  内置本地 Web 服务器写的**访问日志**（存于系统 journal）：来源 IP、时间、请求路径、User-Agent。不离开本机。

**The publisher collects nothing.** No analytics, no telemetry, no crash reporting; no account with
the publisher is required or even possible.
**发布方不收集任何数据**：无埋点、无遥测、无崩溃上报；无需（也无法）注册发布方账号。

## 2. Retention Period / 数据保存期限

- **No tool input and no tool result is ever stored.** Every calculation is discarded when the page
  is closed or reloaded. 工具输入与结果**一律不落盘**，关闭或刷新页面即丢弃。
- The only runtime record is the **access log**. It is held by `systemd-journald` on the device and
  is rotated and expired by the system's own journal policy. There is no developer-side retention
  window, because the publisher operates no server.
  唯一的运行期记录是**访问日志**，由设备上的 `systemd-journald` 保管，按系统 journal 策略轮转与过期。
  **发布方没有服务端**，不存在任何服务端保留期。
- Nothing else is written under `/var/lib/shh1-it-tools` or `/var/log/shh1-it-tools`; those
  directories are created for the service account but hold no user content.
  `/var/lib/shh1-it-tools` 与 `/var/log/shh1-it-tools` 下没有其它写入内容——这两个目录是为服务账号创建的，
  不保存任何用户内容。

## 3. Third-Party Sharing and Data Location / 第三方共享与数据存放地域

- **No third party is involved and no data leaves the device.**
  **不涉及任何第三方，数据不出设备。**
- The application makes **no outbound network request of its own**. All static assets are served
  from inside the package — no CDN, no external font, no external script.
  应用**自身不发起任何出网请求**；全部静态资源由包内提供——无 CDN、无外部字体、无外部脚本。
- The packaged service listens on the loopback interface only (`127.0.0.1:18801`); the external
  entry point is provided by TOS, and this package exposes no additional port.
  包内服务只监听回环地址（`127.0.0.1:18801`）；对外入口由 TOS 提供，本包不额外暴露端口。

| Feature / 功能 | Destination / 去处 | Default / 默认 |
|---|---|---|
| Tool computations / 工具计算 | browser only / 仅浏览器内 | enabled / 启用 |
| Telemetry / 埋点 | none configured / 无 | **absent / 不存在** |
| Outbound requests / 出网请求 | none / 无 | **none / 无** |

- **Data location / 数据存放地域**：the device the user installed the application on. The publisher
  stores no copy anywhere, in any region.
  数据存放于用户安装本应用的设备上；发布方在任何地域都不保存副本。

## 4. Security Measures / 数据安全措施

This is a Deb application. The statements below describe what is actually configured in
`init.d/shh1-it-tools.service` and can be verified there line by line.
本应用是 Deb 应用。以下描述取自 `init.d/shh1-it-tools.service` 的实际配置，可逐条核对：

- Runs as a dedicated **non-root** account with a disabled login shell (`User=shh1ittools`).
  以专用**非 root** 账号运行，登录 shell 已禁用（`User=shh1ittools`）。
- systemd hardening — every item below is actually enabled in the unit / systemd 加固，以下各项均已实际启用：
  `NoNewPrivileges` · `ProtectSystem=strict` · `ProtectHome` · `PrivateTmp` · `PrivateDevices` ·
  `ProtectKernelTunables` · `ProtectKernelModules` · `ProtectControlGroups` · `RestrictRealtime` ·
  `RestrictSUIDSGID` · `LockPersonality` · `RemoveIPC` · `SystemCallArchitectures=native`
- Writable paths are limited to the package directory and `/var/lib/shh1-it-tools`
  (`ReadWritePaths=`); the rest of the file system is read-only to the service.
  可写路径仅限包目录与 `/var/lib/shh1-it-tools`（`ReadWritePaths=`），其余文件系统对服务只读。
- The install / remove scripts perform **no network operation** — no `apt`, no `pip`, no `curl`.
  安装/卸载脚本**不做任何网络操作**——无 `apt`、无 `pip`、无 `curl`。
- No credential or secret is shipped in the package, and no data passes through a
  publisher-operated server. 包内不含任何凭据或密钥；数据不经过发布方服务器。

## 5. User Rights / 用户权利

There is no account system and no user dataset. The administrator of the device can, at any time and
without contacting the publisher / 本应用没有账号体系，也不建立用户数据集。设备管理员可随时（无需联系发布方）：

- **Access / 查阅**：read the access log with `journalctl -u shh1-it-tools`.
- **Erase / 清除**：delete that log — see section 6 — after which no trace of past visits remains on
  the device. 删除该日志（见第 6 节）后，本机不再留有访问痕迹。
- **Opt out / 拒绝采集**：no usage data is collected in the first place, and the application has no
  telemetry to switch off. 本应用从一开始就不采集使用数据，也没有可关闭的埋点开关。

## 6. Deletion Channel / 数据删除途径

1. **Logs / 日志**：`journalctl --vacuum-time=1s -u shh1-it-tools`, or let the system journal policy
   rotate them out. 用该命令清空，或交由系统 journal 策略轮转。
2. **Uninstall with data / 卸载时删除**：uninstall in the TOS App Center and select
   "delete data" (equivalent to `dpkg --purge shh1-it-tools`) — this removes
   `/var/lib/shh1-it-tools`, `/var/log/shh1-it-tools` and `/usr/local/shh1-it-tools` entirely.
   在 TOS 应用中心卸载并勾选「同时删除数据」（等价于 `dpkg --purge shh1-it-tools`）——将完整移除上述三处目录。
3. **Manual / 手动**：delete those three directories on the device.
   在设备上手动删除上述三个目录。

Uninstalling **without** selecting "delete data" keeps `/var/lib/shh1-it-tools` and
`/var/log/shh1-it-tools`, so the app can be reinstalled without losing its service account state.
This is deliberate. There is no publisher-side copy to delete.
**不勾选**「同时删除数据」的普通卸载会保留 `/var/lib/shh1-it-tools` 与 `/var/log/shh1-it-tools`，
便于重装后继续使用——这是有意行为。不存在任何发布方侧的副本需要删除。

## 7. Contact / 联系方式

- **Packaging / 本封装仓库** — installation, uninstallation and TOS-integration questions go here /
  安装、卸载与 TOS 集成相关的问题走这里：
  <https://github.com/RyanYang163/shh1-it-tools/issues>
- **Upstream project / 上游项目** — questions about IT-Tools' features themselves go here /
  IT-Tools 功能本身的问题走这里：
  <https://github.com/CorentinTh/it-tools/issues>

Questions about this policy can be raised in either tracker.
本政策的疑问可在以上任一仓库提出。注意上游仓库由上游作者维护，不负责本 TOS 封装的安装与卸载问题。
