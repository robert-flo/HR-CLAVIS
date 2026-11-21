# OS-HR-CLAVIS — rf-dotfiles#60 (HR-CLAVIS#60)

- **Issue:** https://github.com/robert-flo/HR-CLAVIS/issues/60 (cerrado y sin etiquetas de triage)
- **PR:** https://github.com/robert-flo/rf-dotfiles/pull/74 (squash-merged a `master`)
- **Head SHA:** `c5e1a8b877b87fba0d46e336e21b8cfe63ff9c3f`
- **Merge SHA:** `8e9dc74bac1fc72dc4ad2a795aa7b08e21575526`
- **Merge Danger:** `Door: two-way` / `Blast Radius: Low`

## Cambio realizado
En `.personal/niri/config.kdl` (línea 434), se actualizó el keybinding `Mod+T` de `alacritty` a `ghostty` (tanto `hotkey-overlay-title` como `spawn`), dejando intactos todos los demás atajos (`Mod+Return`, `Mod+Grave`, `Mod+Ctrl+Return`, `Mod+Shift+N`).

## Evidencia de pruebas
- `chezmoi diff --source=/z/repos/rf-dotfiles/60-mod-t-ghostty ~/.config/niri/config.kdl`: mostró el cambio exacto en la línea 434.
- `sudo -u niri-test niri validate -c /home/niri-test/.config/niri/config.kdl`: `INFO niri: config is valid` (exit 0).
- `chezmoi apply` aplicado en `niri` (respaldo previo en `~/.config/niri/config.kdl.bak-20261121-1705` y registro en `/z-home-repos/CAMBIOS.md`), luego sincronizado `/z/rf-dotfiles` a `8e9dc74` y re-aplicado `chezmoi apply --source=/z/rf-dotfiles --force ~/.config/niri/config.kdl`.
- `niri validate` en `niri`: `INFO niri: config is valid` (exit 0).
- `grep 'Mod+T' ~/.config/niri/config.kdl`: `Mod+T hotkey-overlay-title="Open a Terminal: ghostty" { spawn "ghostty"; }`.
- `/code-review`: Standards 0 hallazgos, Spec 0 hallazgos.
- Worktree `/z/repos/rf-dotfiles/60-mod-t-ghostty` eliminado.
