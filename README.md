# IT-Tools

| 项 | 值 |
|---|---|
| 应用 ID | `shh1-it-tools` |
| 形态 | Deb 应用（单包模式） · WebUI 内嵌（iframe） |
| 版本 | 1.0.011 |
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

- 上游为纯静态 SPA。仓库里的 `webui/` 只是占位页；**真正的产物由 CI 在发布时从上游源码构建**：
  clone 上游 tag `v2024.10.22-7ca5933` → `pnpm install` → `BASE_URL=/shh1-it-tools/ pnpm exec vite build`。
- ⚠️ 构建期施加两处平台适配：**`BASE_URL=/shh1-it-tools/`**（让资源/chunk/worker 的 base 进入子路径）
  与 **router 改 hash 模式**（TOS 托管不做 SPA history 回退，深层链接会 404）。
  每处都带 `grep` 门禁，改不到即构建失败。**逐条声明见 [`NOTICE`](./NOTICE)**（GPL-3.0 §5(a)）。
- 🔴 **不要再回到「取上游已构建产物 + sed 打补丁」的老路** —— Vite 的 preload helper 在产物里有
  一份独立于 `import.meta.env.BASE_URL` 的写死 base，补丁修不净，1.0.009 就是这么被判 F7 的。
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
