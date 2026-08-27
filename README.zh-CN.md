<p align="center">
  <img src="assets/branding/dsh-banner.png" alt="DSH Archive Manager" width="100%">
</p>

<div align="center">

  # DSH Archive Manager

  **在 DeepSeek Harness 中安全管理已归档会话**

  [English](README.md) · [更新日志](CHANGELOG.zh-CN.md) · [Apache-2.0](LICENSE)

  [![许可证：Apache-2.0](https://img.shields.io/badge/许可证-Apache--2.0-blue.svg)](LICENSE)
  [![npm](https://img.shields.io/npm/v/%40ggquanta528%2Fdsh-archive-manager)](https://www.npmjs.com/package/@ggtec528/dsh-archive-manager)
  [![GitHub](https://img.shields.io/badge/GitHub-ggquanta528%2Fdsh--archive--manager-0f766e.svg)](https://github.com/ggquanta528/dsh-archive-manager)
  [![DSH Web Plugin](https://img.shields.io/badge/DSH%20Web-Plugin-0f766e.svg)](https://github.com/ggquanta528/dsh-archive-manager)
  [![Node.js 22 or later](https://img.shields.io/badge/Node.js-22%20or%20later-339933.svg?logo=node.js&logoColor=white)](https://nodejs.org/)
</div>

> DSH Archive Manager 是社区维护的 DeepSeek Harness（DSH）插件，并非 DeepSeek AI 官方产品。
>
> **本仓库是 [@michengai/dsh-archive-manager](https://github.com/MichengAI/dsh-archive-manager) 的复刻分支。** 此复刻移除了工作区侧栏中的"删除会话"选项，以避免误操作；永久删除仍可通过「设置 → 已归档」进行。其他所有功能保持不变。

## 功能概览

- 在侧栏会话菜单中选择「归档会话」。
- 在「设置 → 已归档」按工作区查看归档会话，并支持搜索、按更新时间/创建时间/标题排序和按项目筛选。
- 安全取消归档，将会话恢复到原工作区位置。
- 在项目分组中批量恢复或永久删除该项目的全部已归档聊天。
- 在页面顶部一键恢复全部已归档聊天。
- 经确认后永久删除会话、工作区归属、归档标记和投影缓存。
- 经确认后删除全部已归档聊天，包含子代理。
- 已删除的未加载归档会话会立即从已连接客户端的侧栏移除。
- **本复刻特性：** 工作区侧栏中不再显示「删除会话」选项，以避免误操作；永久删除可通过「设置 → 已归档」进行。

## 界面预览

在侧栏会话菜单中选择「归档会话」：

![从会话菜单归档会话](assets/screenshots/archive-session-menu.png)

在「设置 → 已归档」中搜索、排序、按项目筛选、取消归档或永久删除：

![已归档聊天设置页面](assets/screenshots/archived-sessions.png)

## DSH 产品生态

本产品既可以独立安装，也可以随桌面端或 Web 套件一起使用。它们共享同一个 DSH 核心，但面向不同的使用方式：

| 产品 | 与本产品的关系 |
| --- | --- |
| [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness) | 本产品的运行宿主，提供模型、会话、工具和插件系统 |
| [DSH Codex Desktop](https://github.com/MichengAI/dsh-codex-desktop) | 下载安装即用的桌面产品，已内置本产品和其他 5 个功能产品 |
| [DSH Codex Suite](https://github.com/MichengAI/dsh-codex-ui/tree/main/packages/dsh-codex-suite) | 面向已有 DSH Web 环境的一键套件，会安装本产品和其他 5 个功能产品 |
| 6 个功能产品 | [Codex UI](https://github.com/MichengAI/dsh-codex-ui) · [IM Connect](https://github.com/MichengAI/dsh-im-connect) · [Automation](https://github.com/MichengAI/dsh-automation) · [Skills Manager](https://github.com/MichengAI/dsh-skills-manager) · [Archive Manager](https://github.com/ggquanta528/dsh-archive-manager) · [Agency Agents](https://github.com/MichengAI/dsh-agency-agents) |

## 前置条件

- 已可正常运行 DeepSeek Harness Web，且可在 PowerShell 中使用 `dsh`。
- 以下示例使用 `web` profile；请替换为实际目标 profile。
- 二次开发需要 Node.js 22+ 与 pnpm。

## 安装

### 从 npm 安装（推荐）

```powershell
[Console]::OutputEncoding = [System.Text.Encoding]::UTF8
$OutputEncoding = [System.Text.Encoding]::UTF8
dsh plugin --profile web add @ggtec528/dsh-archive-manager
dsh --profile web --dump-config
```

完成后重启 DSH Web 并硬刷新浏览器。配置输出中应包含 `workspace-archive-manager` 与 `ui-workspace-archive-manager`。

### 从源码安装

克隆仓库并以本地插件方式安装：

```powershell
[Console]::OutputEncoding = [System.Text.Encoding]::UTF8
$OutputEncoding = [System.Text.Encoding]::UTF8
git clone https://github.com/ggquanta528/dsh-archive-manager.git
Set-Location .\dsh-archive-manager
pnpm install --frozen-lockfile
pnpm build
dsh plugin --profile web add .
dsh --profile web --dump-config
```

### 从本地目录安装（开发用）

```powershell
[Console]::OutputEncoding = [System.Text.Encoding]::UTF8
$OutputEncoding = [System.Text.Encoding]::UTF8
Set-Location /path/to/dsh-archive-manager
pnpm install --frozen-lockfile
pnpm build
dsh plugin --profile web add .
dsh --profile web --dump-config
```

未把 `dsh` 装进 PATH 时，把开头的 `dsh` 换成 `npx --yes @deepseek-ai/dsh`。

## 使用

1. 在侧栏右键或打开会话菜单，选择「归档会话」。
2. 打开「设置 → 已归档」，按工作区查看归档会话。
3. 按标题搜索，按更新时间、创建时间或标题排序，或按项目筛选列表。
4. 点击「取消归档」恢复单个会话，或在顶部点击「全部恢复」。
5. 打开项目标题右侧菜单，可恢复或删除该项目的全部已归档聊天。
6. 如需永久删除单个会话，请前往「设置 → 已归档」，找到会话后点击删除图标。**删除无法撤销。**

安装或升级后找不到入口时，重启 DSH Web 并硬刷新浏览器；入口位于「设置」中，连接器之后。

## 数据处理边界

- 删除操作始终需要确认。
- 删除会移除会话目录、工作区记录、归档集合和投影缓存。
- 正在写入的会话会在完成写入后清理，避免截断数据。
- 本插件替换 DSH 默认的工作区和会话投影服务；请仅通过 DSH profile 安装，避免手工拼接补丁配置。

## 二次开发

当前仓库未提供 `src` 源目录，`lib` 是直接维护的运行源码；这是当前仓库的实现方式，不是新插件的推荐布局。新插件建议使用 `src` 开发并构建到 `lib`：

- [lib\index.js](lib/index.js)：客户端插件 Host 服务入口。
- [lib\workspace.js](lib/workspace.js)：归档会话和工作区服务实现。
- [lib\projcache.js](lib/projcache.js)：会话投影缓存实现。
- [lib\client.js](lib/client.js)：设置页和归档会话界面。
- `test\*.test.mjs`：Host、客户端、Remote 和样式边界测试。

修改后运行检查、测试并用本地目录重新安装：

```powershell
[Console]::OutputEncoding = [System.Text.Encoding]::UTF8
$OutputEncoding = [System.Text.Encoding]::UTF8
pnpm build
pnpm test
pnpm pack:check
dsh plugin --profile web add .
```

`pnpm build` 负责发布包完整性检查，不会将 `lib` 重新编译为其他目录。

## 验证

```powershell
[Console]::OutputEncoding = [System.Text.Encoding]::UTF8
$OutputEncoding = [System.Text.Encoding]::UTF8
pnpm build
pnpm test
pnpm pack:check
```

`prepublishOnly` 会在发布前执行构建检查与测试。

## 许可证

本项目采用 [Apache License 2.0](LICENSE)。

## 更新日志

完整保留的发布说明见 [CHANGELOG.zh-CN.md](CHANGELOG.zh-CN.md)。
