# opencode 配置自动安装向导

> 本文件是给 **opencode agent** 读取并执行的安装指南。
> 在新电脑上克隆本仓库后进入仓库目录启动 opencode，本文件会通过 `.opencode/opencode.json` 的 `instructions` 自动加载为系统指令。用户只需说一句"开始安装"，agent 即按本指南逐步执行。

## 目标

把本仓库中的全局配置安装到新电脑的 `~/.config/opencode/`（Windows 为 `%USERPROFILE%\.config\opencode\`），使新电脑的 opencode 在插件、模型、TUI 设置上与源电脑保持一致。

需要安装到目标目录的文件映射：

| 仓库内文件 | 目标位置 |
|---|---|
| `opencode.global.jsonc` | `~/.config/opencode/opencode.jsonc` |
| `tui.json` | `~/.config/opencode/tui.json` |
| `package.json` + 锁文件 | `~/.config/opencode/` |

## 安装步骤（严格按顺序执行，任何一步失败先向用户报告，不得跳过）

### 第 1 步：检查前提

1. 运行 `opencode --version`，确认 opencode 已安装；若未安装，告诉用户先安装 opencode，然后停止。
2. 确认当前工作目录就是本仓库根目录（能找到本 SETUP.md 文件的目录）。
3. 检查目标目录：如果 `~/.config/opencode/` 已存在，先把旧目录重命名为 `~/.config/opencode.bak` 备份，再继续。

### 第 2 步：安装本地插件依赖

`opencode.jsonc` 中引用了 `file://./node_modules/@tarquinen/opencode-dcp/dist/index.js`——这是**本地文件引用**，不复制 node_modules，依赖必须重新安装：

1. 将 `package.json`、`bun.lock`、`package-lock.json` 复制到 `~/.config/opencode/`。
2. 在 `~/.config/opencode/` 下执行依赖安装（**先试 bun，没有 bun 用 npm**）：
   - 有 bun：`bun install`
   - 无 bun：`npm install`
3. 安装成功后确认 `~/.config/opencode/node_modules/@tarquinen/opencode-dcp/dist/index.js` 存在。

### 第 3 步：复制配置文件

1. 将 `opencode.global.jsonc` 复制为 `~/.config/opencode/opencode.jsonc`。
2. 将 `tui.json` 复制为 `~/.config/opencode/tui.json`。

### 第 4 步：验证安装

1. 确认 `~/.config/opencode/` 下存在：`opencode.jsonc`、`tui.json`、`package.json`、`node_modules/`。
2. 告诉用户重启 opencode（配置只在启动时加载）。
3. 重启后检查插件是否加载成功：`oh-my-openagent`（npm 自动安装）与 `opencode-dcp`（本地引用）应无报错。

### 第 5 步：登录模型提供商（需要用户交互）

1. 告知用户执行 `opencode auth login` 并完成浏览器登录。
2. 说明原因：登录凭据保存在 `~/.local/share/opencode/auth.json`（Windows 为 `%USERPROFILE%\.local\share\opencode\auth.json`），出于安全**不进入 git 仓库**，每台新电脑必须手动登录一次，无法自动迁移。

## 硬性约束

- **绝不**复制、提交或上传：`auth.json`、任何 Token、API Key、`.env` 文件。
- 安装过程中若 `~/.config/opencode/opencode.jsonc` 已存在，先备份再覆盖，禁止直接删除。
- 安装完成后向用户做一次总结：装了什么、验证结果如何、还差什么（登录）。
