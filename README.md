# opencode-dotfiles

同步 opencode 全局配置（插件、模型、TUI 设置）到多台电脑的 GitHub 仓库。新电脑上的 opencode 可**自动读取并执行**仓库内的 `SETUP.md` 完成自安装。

## 仓库内容

| 文件 | 说明 |
|---|---|
| `opencode.global.jsonc` | 全局配置（安装时复制为 `~/.config/opencode/opencode.jsonc`） |
| `tui.json` | TUI 界面配置（提示音、通知等） |
| `package.json` / `bun.lock` / `package-lock.json` | 本地插件 `@tarquinen/opencode-dcp` 的依赖声明 |
| `SETUP.md` | **自动安装向导**——新电脑的 opencode agent 按此文件逐步安装 |
| `.opencode/opencode.json` | 项目配置：自动把 `SETUP.md` 加载为系统指令 |

## 新电脑安装（一行起步）

```bash
# 1. 安装 opencode（https://opencode.ai/docs/）
# 2. 克隆仓库并进入目录
git clone https://github.com/<你的用户名>/opencode-dotfiles.git
cd opencode-dotfiles
# 3. 启动 opencode
opencode
```

opencode 启动后会自动加载 `SETUP.md`，对 opencode 说一句 **"开始安装"**，agent 就会自动完成：安装本地插件依赖 → 复制配置文件到 `~/.config/opencode/` → 验证 → 引导你登录模型提供商。

最后手动执行一次 `opencode auth login`（凭据不进 git，每台电脑需登录一次），然后重启 opencode 即可。

> 备选方式：在任何目录启动 opencode，直接说"克隆 https://github.com/<你的用户名>/opencode-dotfiles 并按照 SETUP.md 执行安装"，agent 会自己完成克隆和安装。

## 更新配置（源电脑 → GitHub）

```bash
cd opencode-dotfiles
# 把 ~/.config/opencode/opencode.jsonc 的修改同步进来（或直接改 opencode.global.jsonc）
git add .
git commit -m "update: 说明改动内容"
git push
```

新电脑更新：

```bash
cd opencode-dotfiles
git pull
```

然后重新进入目录启动 opencode，说"按照 SETUP.md 更新配置"。

## 安全说明

- `auth.json`、Token、API Key **永不入库**。`opencode auth login` 的凭据在 `~/.local/share/opencode/auth.json`。
- 仓库建议设为 **private**。
