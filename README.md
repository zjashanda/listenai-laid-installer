# listenai-laid-installer

Codex skill for installing the `laid` command on Windows PowerShell and Linux bash/zsh.

The installed `laid` command lists:

- stable ListenAI USB device keys
- direction (`Render` / `Capture`)
- channel count
- friendly device name
- endpoint id on Windows

## Skill layout

- `SKILL.md`: skill instructions
- `agents/openai.yaml`: Codex UI metadata
- `scripts/install_laid_windows.ps1`: install/update `laid` for PowerShell
- `scripts/install_laid_linux.sh`: install/update `laid` for bash/zsh

## What `laid` does

### Windows

Installs a PowerShell function into the user's profile and reads active ListenAI endpoints from MMDevices plus PnP mapping.

Example usage:

```powershell
laid
laid Render
laid Capture
laid -Json
```

### Linux

Installs a shell function into `~/.bashrc` and/or `~/.zshrc`, then scans `/dev/snd`, `udevadm`, and `/proc/asound/card*/stream*`.

Example usage:

```bash
laid
```

## Install the skill

Copy this folder into:

```text
~/.codex/skills/listenai-laid-installer
```

Then restart Codex.

## Install `laid`

### Windows

```powershell
powershell -ExecutionPolicy Bypass -File .\scripts\install_laid_windows.ps1
```

### Linux

```bash
bash ./scripts/install_laid_linux.sh
```

## Notes

- Windows channel counts are parsed from the endpoint mix format blob.
- Linux channel counts are best-effort values parsed from ALSA stream info.
