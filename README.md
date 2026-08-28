<p align="center">
  <img src="assets/branding/dsh-banner.png" alt="DSH Archive Manager" width="100%">
</p>

<div align="center">

  # DSH Archive Manager

  **Safely manage archived sessions in DeepSeek Harness**

  [简体中文](README.zh-CN.md) · [Changelog](CHANGELOG.md) · [Apache-2.0](LICENSE)

  [![License: Apache-2.0](https://img.shields.io/badge/License-Apache--2.0-blue.svg)](LICENSE)
  [![npm](https://img.shields.io/npm/v/%40ggtec528%2Fdsh-archive-manager)](https://www.npmjs.com/package/@ggtec528/dsh-archive-manager)
  [![GitHub](https://img.shields.io/badge/GitHub-ggquanta528%2Fdsh--archive--manager-0f766e.svg)](https://github.com/ggquanta528/dsh-archive-manager)
  [![DSH Web Plugin](https://img.shields.io/badge/DSH%20Web-Plugin-0f766e.svg)](https://github.com/ggquanta528/dsh-archive-manager)
  [![Node.js 22 or later](https://img.shields.io/badge/Node.js-22%20or%20later-339933.svg?logo=node.js&logoColor=white)](https://nodejs.org/)
</div>

> DSH Archive Manager is a community-maintained DeepSeek Harness (DSH) plugin, not an official DeepSeek AI product.
>
> **Fork of [@michengai/dsh-archive-manager](https://github.com/MichengAI/dsh-archive-manager).** This fork removes the "Delete Session" option from the workspace sidebar to prevent accidental deletion; permanent deletion is still available through **Settings → Archived**. All other functionality is preserved.

## Features

- Archive a session from the sidebar session menu.
- Search archived chats, sort them by update time, creation time, or title, and filter them by workspace in **Settings → Archived**.
- Restore a session to its original workspace with **Unarchive**.
- Restore or permanently delete every archived chat in a project group.
- Restore all archived chats from the page header.
- Permanently delete a confirmed session, its workspace association, archive marker, and projection cache.
- Delete all archived chats after confirmation, including child agents.
- Remove unloaded deleted sessions from connected sidebars immediately.
- **Fork difference:** The "Delete Session" option is removed from the workspace sidebar to prevent accidental deletion. Permanent deletion is still available through **Settings → Archived**.

## Screenshots

Search, sort, filter by project, unarchive, or permanently delete chats in **Settings → Archived**:

![Archived chats settings page](assets/screenshots/archived-sessions.png)

## DSH product ecosystem

This product can be installed independently or used through the desktop app or Web suite. They share the same DSH core but serve different ways of working:

| Product | Relationship to this product |
| --- | --- |
| [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness) | The host runtime that provides models, sessions, tools, and the plugin system |
| [DSH Codex Desktop](https://github.com/MichengAI/dsh-codex-desktop) | A ready-to-install desktop product with this product and the other five feature products built in |
| [DSH Codex Suite](https://github.com/MichengAI/dsh-codex-ui/tree/main/packages/dsh-codex-suite) | A one-click suite for existing DSH Web environments that installs this product and the other five feature products |
| Six feature products | [Codex UI](https://github.com/MichengAI/dsh-codex-ui) · [IM Connect](https://github.com/MichengAI/dsh-im-connect) · [Automation](https://github.com/MichengAI/dsh-automation) · [Skills Manager](https://github.com/MichengAI/dsh-skills-manager) · [Archive Manager](https://github.com/ggquanta528/dsh-archive-manager) · [Agency Agents](https://github.com/MichengAI/dsh-agency-agents) |

## Prerequisites

- A working DeepSeek Harness Web installation with `dsh` available in PowerShell.
- Examples use the `web` profile; replace it with the target profile.
- Development requires Node.js 22+ and pnpm.

## Installation

### Install from npm (recommended)

```powershell
[Console]::OutputEncoding = [System.Text.Encoding]::UTF8
$OutputEncoding = [System.Text.Encoding]::UTF8
dsh plugin --profile web add @ggtec528/dsh-archive-manager
dsh --profile web --dump-config
```

Restart DSH Web and hard-refresh the browser. The configuration output should contain `workspace-archive-manager` and `ui-workspace-archive-manager`.

### Install from source

Clone the repository and install it as a local plugin:

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

### Install from local directory (for development)

```powershell
[Console]::OutputEncoding = [System.Text.Encoding]::UTF8
$OutputEncoding = [System.Text.Encoding]::UTF8
Set-Location /path/to/dsh-archive-manager
pnpm install --frozen-lockfile
pnpm build
dsh plugin --profile web add .
dsh --profile web --dump-config
```

If `dsh` is not on PATH, replace the leading `dsh` with `npx --yes @deepseek-ai/dsh`.

## Usage

1. Open the sidebar session menu and choose **Archive session**.
2. Open **Settings → Archived** to inspect sessions by workspace.
3. Search by title, sort by update time, creation time, or title, or filter the list by project.
4. Select **Unarchive** to restore one session, or select **Restore all** in the page header.
5. Open a project heading's action menu to restore or delete all of that project's archived chats.
6. To permanently delete a session, go to **Settings → Archived**, find the session, and use the delete icon. **It cannot be undone.**

If the entry is missing after installation or upgrade, restart DSH Web and hard-refresh the browser. It is located directly after **Connectors** in Settings.

## Data handling limits

- Deletion always requires confirmation.
- It removes the session directory, workspace records, archive set, and projection cache.
- A live session finishes writing before cleanup to prevent data truncation.
- The plugin replaces DSH’s default workspace and projection services. Install through the DSH profile instead of manually composing the patch.

## Secondary development

This repository has no `src` directory. `lib` is directly maintained runtime source, which is its current layout rather than the recommended layout for new plugins. New plugins should prefer `src` built to `lib`.

- [lib\index.js](lib/index.js): host service entry point.
- [lib\workspace.js](lib/workspace.js): archived-session and workspace service.
- [lib\projcache.js](lib/projcache.js): session projection cache.
- [lib\client.js](lib/client.js): Settings page and archive UI.
- `test\*.test.mjs`: host, client, Remote, and styling coverage.

After changing the runtime source, validate, test, and install from the local directory:

```powershell
[Console]::OutputEncoding = [System.Text.Encoding]::UTF8
$OutputEncoding = [System.Text.Encoding]::UTF8
pnpm build
pnpm test
pnpm pack:check
dsh plugin --profile web add .
```

`pnpm build` validates package integrity; it does not compile `lib` into another directory.

## Validation

```powershell
[Console]::OutputEncoding = [System.Text.Encoding]::UTF8
$OutputEncoding = [System.Text.Encoding]::UTF8
pnpm build
pnpm test
pnpm pack:check
```

`prepublishOnly` runs the build check and tests before publishing.

## License

Licensed under [Apache License 2.0](LICENSE).

## Changelog

See [CHANGELOG.md](CHANGELOG.md) for retained release notes.

## Acknowledgments

Thanks to [@michengai/dsh-archive-manager](https://github.com/MichengAI/dsh-archive-manager) for the original repository that this project was forked from.
