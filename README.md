# listenai-laid-installer

安装或更新用于 ListenAI 测试的 `laid` 命令，并在 Windows 提供通用声卡端点列表 `audio-list`。当需要设置、修复或验证 PowerShell/bash/zsh 的 `laid`，查询 Windows 声卡端点、稳定 USB 设备标识和通道数，或迁移、说明本地命令工作方式时使用。

## Skill layout

- `agents/openai.yaml`
- `scripts/install_laid_linux.sh`
- `scripts/install_laid_windows.ps1`
- `SKILL.md`

## Install the skill

Copy this folder into:

```text
~/.codex/skills/listenai-laid-installer
```

Then restart Codex.

## Usage and workflow

在用户的 shell profile 中安装 `laid` 命令，并验证它能显示 ListenAI 设备的稳定标识和通道数。Windows 同时安装通用声卡端点命令 `audio-list`。

## 命令说明

- `laid`：仅列出名称为 `ListenAI Audio` 的活动端点，供 ListenAI 测试脚本稳定选择设备。
- `audio-list`：仅限 Windows，默认列出所有活动采集/播放端点，包含方向、状态、通道数、名称、端点 ID 与 USB 稳定标识。
- `audio-list -Direction Capture`：仅列出采集端点。
- `audio-list -IncludeDisabled`：同时包含禁用、未连接和不存在的端点。
- `audio-list -Json`：输出 JSON，便于脚本调用。

`audio-list` 中的 USB 设备会尽量格式化为 `VID_xxxx&PID_xxxx:<稳定尾标识>`；非 USB 设备保留其 Windows 接口标识。

## 工作流程

1. 识别目标平台和 shell 类型。
2. 执行 `scripts/` 中对应的安装脚本。
3. 重新加载已修改的 profile，或明确告知用户需要执行的加载命令。
4. 验证 `laid` 可解析且返回 ListenAI 设备；在 Windows 验证 `audio-list` 能返回通用端点和通道数。

## 平台说明

- **Windows PowerShell**：执行 `scripts/install_laid_windows.ps1`。默认使用 `CurrentUserAllHosts`，使当前用户的全部 PowerShell host 都可使用这些命令；仅在用户明确要求 host 专用配置时使用 `-Scope CurrentUserCurrentHost`。
- **Linux bash/zsh**：执行 `scripts/install_laid_linux.sh`。默认同时更新 `~/.bashrc` 和 `~/.zshrc`；传入 `bash` 或 `zsh` 可只更新指定 shell。Linux 的通道数从 `/proc/asound/card*/stream*` 解析，属于尽力而为的结果。

## 验证

- **Windows**：默认安装后执行 `. $PROFILE.CurrentUserAllHosts`，或打开新的 PowerShell 会话；使用 `Get-Command laid,audio-list`、`laid` 和 `audio-list` 验证。
- **Linux**：执行 `source ~/.bashrc` 或 `source ~/.zshrc`，也可打开新的 shell；使用 `command -v laid` 和 `laid` 验证。

## 编辑规则

- 保留 profile 文件中与本工具无关的用户内容。
- 仅替换 `# >>> laid >>>` 与 `# <<< laid <<<` 之间的内容。
- 保持安装器幂等，重复执行时应刷新函数，不得重复追加。
- `laid` 与 `audio-list` 都是写入用户 profile/rc 文件的 shell 函数，不会创建系统级可执行文件。

## 资源

- `scripts/install_laid_windows.ps1`：安装/更新 PowerShell 的 `laid`、`audio-list`、`Get-ListenAIDeviceKeys` 和 `Get-AudioEndpointList`，并从端点格式数据读取通道数。
- `scripts/install_laid_linux.sh`：安装/更新 bash/zsh 的 `laid`，从 ALSA stream 信息尽力解析通道数。
