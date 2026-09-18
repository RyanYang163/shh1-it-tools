# IT-Tools

| 项 | 值 |
|---|---|
| 应用 ID | `shh1-it-tools` |
| 形态 | Deb 应用（单包模式） · WebUI 内嵌（iframe） |
| 版本 | 1.0.0 |
| 上游项目 | https://github.com/CorentinTh/it-tools |
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

- 上游为纯静态 SPA，构建产物放入 webui/ 后由 build.sh 打包为 webui.bz2。
- 替换 webui/index.html 为上游 `npm run build` 的 dist 内容即可。
- 本应用无数据库、无后端，是 9 个应用中最容易过审的一个。
- [ ] 真机安装、启动、停止、卸载残留四项实测
- [ ] 首屏加载 ≤ 5 秒（指引 H10）
- [ ] x86_64 与 aarch64 分别构建并测试（指引 H7）
- [ ] 提交前跑一遍指引 13.9 上架前自查清单

## 隐私政策

见 [PRIVACY.md](./PRIVACY.md)（对应审核项 C3–C8）。

## 许可证与出处

本仓库**仅包含 TOS 平台集成所需的配置文件与打包脚本**，应用本体的源码与二进制来自上游项目：https://github.com/CorentinTh/it-tools

上游许可证：**%s**。本封装保留上游许可证声明，未修改上游代码（Deb 形态下按上游许可证要求随包提供 LICENSE）。

应用名称与图标为上游项目的标识；本仓库图标为自行绘制的简易图形，不含上游商标元素（对应审核项 H19）。
