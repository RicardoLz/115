# 115-us · GitHub 编译发布壳仓库

本仓库**不存放源码**，仅承载 GitHub Actions 工作流，作为 115US 的编译发布出口。

## 架构

| 环节 | 位置 |
|---|---|
| 源码唯一真源 | `gitgud.io/RicardoLz/115-us`（master） |
| 编译发布 | 本仓库 GitHub Actions（`build-release`） |
| 发布产物 | GitHub Release（zip = 编译出的 exe + dll + 运行配置） |

## 工作流行为（.github/workflows/build-release.yml）

- **手动**：Actions 页面 → `build-release` → Run workflow。
- **定时**：每天 09:00（北京时间）自动运行。
- **更新检测**：读取 gitgud master 的 HEAD；若该提交已构建过（Release tag = `us-<完整sha>`）则跳过，否则拉取源码编译并发布。
- **产物**：服务端 Go 程序（wireprobe-pvf/dfolauncher/admin/dfo-tool/gmtool）+ 探针 probe.exe + r7 客户端补丁 DLL（dinput8/ChineseLocalization 与 7 个 `.115us-mods` 插件）+ 服务端运行配置 `configs/**`，打包为 zip 挂到 Release。

> 源码更新流程：在 gitgud 提交 → 等下一次定时（或手动触发）→ GitHub 自动编译发布，无需推 GitHub。
