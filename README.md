# listenai-laid-installer

Install or refresh the `laid` shortcut command that lists stable USB audio device keys and channel counts for ListenAI testing. Use when Codex needs to set up, repair, or verify `laid` on Windows PowerShell or Linux bash/zsh, migrate the helper to another machine, or explain how the local installation works.

## Skill layout

- `SKILL.md`
- `agents/openai.yaml`
- `scripts/install_laid_linux.sh`
- `scripts/install_laid_windows.ps1`

## Install the skill

Copy this folder into:

```text
~/.codex/skills/listenai-laid-installer
```

Then restart Codex.

## Usage and workflow

Install the `laid` shell command in the user's local profile files, then verify it works and shows channel counts.

## Workflow

1. Detect the target platform and shell family.
2. Run the matching installer from `scripts/`.
3. Reload the modified profile or tell the user exactly what to reload.
4. Verify `laid` resolves and returns device keys.

## Platform guide

- **Windows PowerShell**: Run `scripts/install_laid_windows.ps1`. Default to `CurrentUserAllHosts` so the helper is available in every PowerShell host for the current user. Use `-Scope CurrentUserCurrentHost` only when the user wants host-specific installation.
- **Linux bash/zsh**: Run `scripts/install_laid_linux.sh`. By default it updates both `~/.bashrc` and `~/.zshrc`. Pass `bash` or `zsh` to target only one shell. Linux channel counts are best-effort values parsed from `/proc/asound/card*/stream*`.

## Validation

- **Windows**:
  - Reload with `. $PROFILE.CurrentUserAllHosts` after default install, or tell the user to open a new PowerShell session.
  - Verify with `Get-Command laid` and `laid`.
- **Linux**:
  - Reload with `source ~/.bashrc` or `source ~/.zshrc`, or tell the user to open a new shell.
  - Verify with `command -v laid` and `laid`.

## Editing rules

- Preserve unrelated user content in profile files.
- Replace only the block between `# >>> laid >>>` and `# <<< laid <<<`.
- Keep the installers idempotent so repeated execution refreshes the helper instead of duplicating it.
- If the user asks how the local installation works, explain that `laid` is installed by writing a shell function into the user's profile/rc file rather than by creating a system-wide executable.

## Resources

- `scripts/install_laid_windows.ps1`: Install/update the PowerShell `laid` and `Get-ListenAIDeviceKeys` functions, including channel-count parsing from the endpoint mix format blob.
- `scripts/install_laid_linux.sh`: Install/update the shell `laid` function in `~/.bashrc` and/or `~/.zshrc`, including best-effort channel parsing from ALSA stream info.
