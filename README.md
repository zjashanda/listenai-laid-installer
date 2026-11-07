# listenai-laid-installer

已降级的文档入口：说明 `laid` / `audio-list` 的安装方式与验证步骤，并指向 `listenai-play` 里的唯一安装脚本。Use when 需要安装、修复或验证 PowerShell/bash/zsh 的 `laid`，查询 Windows 声卡端点与稳定 USB 标识时；实际执行请改用 `listenai-play`。

## Skill layout

- `SKILL.md`
- `agents/openai.yaml`

## Install the skill

Copy this folder into:

```text
~/.codex/skills/listenai-laid-installer
~/.config/opencode/skills/listenai-laid-installer
```

Then restart Codex or OpenCode.

## Usage and workflow

**本仓库已不再提供安装脚本副本。** 安装脚本的唯一来源是 [`zjashanda/listenai-play`](https://github.com/zjashanda/listenai-play)：

- Windows：`listenai-play/scripts/install_laid_windows.ps1`（读取同目录 `audio_endpoints.ps1` 安装 `laid` 与 `audio-list`）
- Linux bash/zsh：`listenai-play/scripts/install_laid_linux.sh`

能力台账中本仓库 `status=archived`、`replaced_by=listenai-play`。保留本仓库只为历史文档可读；新增用法请写到 `listenai-play`。

## 命令说明

- `laid`：仅列出名称为 `ListenAI Audio` 的活动端点，供 ListenAI 测试脚本稳定选择设备。
- `audio-list`：仅限 Windows，默认列出所有活动采集/播放端点，包含方向、状态、通道数、名称、端点 ID 与 USB 稳定标识。
- `audio-list -Direction Capture`：仅列出采集端点。
- `audio-list -IncludeDisabled`：同时包含禁用、未连接和不存在的端点。
- `audio-list -Json`：输出 JSON，便于脚本调用。

`audio-list` 中的 USB 设备会尽量格式化为 `VID_xxxx&PID_xxxx:<稳定尾标识>`；非 USB 设备保留其 Windows 接口标识。

## 怎么安装

先准备 `listenai-play`（`~/.codex/skills/listenai-play` 或同级的 `listenai-play/` 目录），再执行：

```powershell
# Windows PowerShell
powershell -ExecutionPolicy Bypass -File <listenai-play>/scripts/install_laid_windows.ps1
. $PROFILE.CurrentUserAllHosts
Get-Command laid,audio-list
laid
audio-list -Direction Capture
```

```bash
# Linux bash/zsh
bash <listenai-play>/scripts/install_laid_linux.sh
source ~/.bashrc   # 或 source ~/.zshrc
command -v laid && laid
```

Windows 默认写入 `CurrentUserAllHosts`，重复执行只刷新 `# >>> laid >>>` / `# <<< laid <<<` 标记块，不重复追加，也不动标记块外的用户内容。

## 平台说明

- **Windows PowerShell**：`install_laid_windows.ps1` 默认写 `CurrentUserAllHosts`，使当前用户的全部 PowerShell host 都能用；仅在用户明确要求 host 专用配置时传 `-Scope CurrentUserCurrentHost`。
- **Linux bash/zsh**：`install_laid_linux.sh` 默认同时更新 `~/.bashrc` 与 `~/.zshrc`；传 `bash` 或 `zsh` 可只更新指定 shell。Linux 的通道数从 `/proc/asound/card*/stream*` 解析，属尽力而为。

## 验证

- **Windows**：`Get-Command laid,audio-list` 能解析；`laid` 返回 `ListenAI Audio` 端点；`audio-list -Direction Capture` 能看到麦克风通道数。
- **Linux**：`command -v laid` 有输出；`laid` 能列出 ListenAI 端点（通道数来自 `/proc/asound/card*/stream*`，属尽力而为）。

## 编辑规则（迁移提示）

这些规则现在由 `listenai-play` 的安装脚本保证，改脚本请改那边：

- 仅替换标记块内的内容，保留 profile 中与本工具无关的用户内容；
- 安装器必须幂等，重复执行只能刷新函数，不得重复追加；
- `laid` 与 `audio-list` 是写入用户 profile/rc 的 shell 函数，不创建系统级可执行文件。

## 资源

- 安装脚本：`listenai-play/scripts/install_laid_windows.ps1`、`listenai-play/scripts/install_laid_linux.sh`
- 端点查询实现：`listenai-play/scripts/audio_endpoints.ps1`、`listenai-play/scripts/windows_audio.py`
