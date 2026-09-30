# IT-Tools

| 项 | 值 |
|---|---|
| 应用 ID | `shh1-it-tools` |
| 形态 | Deb 应用（单包模式） · WebUI 内嵌（iframe） |
| 版本 | 1.0.007 |
| 上游项目 | https://github.com/CorentinTh/it-tools |
| 上游作者 | Corentin Thomasset（GitHub: [CorentinTh](https://github.com/CorentinTh)） |
| 上游许可证 | GPL-3.0 |
| 宿主端口 | 18801 |

## 简介

面向开发者的在线工具箱：JSON 格式化、Base64、UUID、哈希、时间戳、正则测试等 80 余项。

## 打包

```bash
./build.sh                # 默认 x86_64
./build.sh aarch64        # ARM（Deb 应用）
```

产物在 `build/output/`，同级生成 `<包名>.sha256`。

## 提交前必办事项

- 上游为纯静态 SPA。仓库里的 `webui/` 只是占位页；**真正的产物由 CI 在发布时**从上游官方镜像
  `corentinth/it-tools:2024.10.22-7ca5933` 的 `/usr/share/nginx/html` 提取，
  再把资源绝对路径改写成相对路径，最后才由 `build.sh` 打包成 `webui.bz2`（见 `.github/workflows/release.yml`）。
- 本应用无数据库、无后端，是 9 个应用中最容易过审的一个。
- [ ] 真机安装、启动、停止、卸载残留四项实测
- [ ] 首屏加载 ≤ 5 秒（指引 H10）
- [ ] x86_64 与 aarch64 分别构建并测试（指引 H7）
- [ ] 提交前跑一遍指引 13.9 上架前自查清单

## 隐私政策

见 [PRIVACY.md](./PRIVACY.md)（对应审核项 C3–C8）。

## 许可证与出处

本仓库**仅包含 TOS 平台集成所需的配置文件与打包脚本**；应用本体的代码与界面来自上游项目，
作者为 **Corentin Thomasset（GitHub: [CorentinTh](https://github.com/CorentinTh)）**：
<https://github.com/CorentinTh/it-tools>（锁定 tag `2024.10.22-7ca5933`）。

上游许可证：**GPL-3.0**。本封装以同款许可证发布，保留上游许可证全文（随包提供 `LICENSE`），
并在 [`NOTICE`](./NOTICE) 中列明版权归属与本封装对上游产物做过的改动。

> 本封装**未改动上游源代码**；唯一改动是把上游构建产物里的资源引用由绝对路径改为相对路径
> （`/assets/…` → `./assets/…`），以适配 TOS 反代子路径。完整声明见 [`NOTICE`](./NOTICE)。

应用名称「IT-Tools」为指示性使用，仅用于指明所封装的上游软件；
本仓库图标为自行绘制的简易图形，不含上游商标或 Logo 元素（对应审核项 H19）。
