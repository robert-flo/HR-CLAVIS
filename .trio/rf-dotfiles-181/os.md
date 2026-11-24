# OS Reporte — robert-flo/HR-CLAVIS#181

- **PR:** https://github.com/robert-flo/rf-dotfiles/pull/126
- **Head SHA:** `f188d25b384d684a357a5734e069ce8cef75726a`
- **Merge Commit SHA:** `ef0b0086e2683034d202ae81dd53b2befd04d891`
- **Merge Danger:** `Door: two-way` / `Blast Radius: Low`

## Cambios
- `dot_config/spotify-flags.conf`: agregado con `--ozone-platform=wayland`.
- `.chezmoiignore.tmpl`: agregado `.config/spotify-flags.conf` dentro del bloque `clavis-upstream` junto a `.config/spicetify/**`.
- `AGENTS.md`: documentado `dot_config/spotify-flags.conf` en `Herramientas Comunes`.

## Evidencia de pruebas
- `sudo -u niri-test -H chezmoi diff --source=/z-home-repos/HR-CLAVIS/repos/rf-dotfiles/181-spotify-flags-wayland /home/niri-test/.config/spotify-flags.conf` mostró la creación del archivo con `--ozone-platform=wayland`.
- `sudo -u niri-test -H chezmoi apply --source=/z-home-repos/HR-CLAVIS/repos/rf-dotfiles/181-spotify-flags-wayland --force /home/niri-test/.config/spotify-flags.conf` aplicado y verificado con `cat /home/niri-test/.config/spotify-flags.conf` (`--ozone-platform=wayland`).
- Aplicado en `niri` (`~/.config/spotify-flags.conf` -> `--ozone-platform=wayland`).
- `CHEZMOI_PROFILE=clavis-upstream chezmoi managed ... | grep -c spotify-flags.conf` devolvió `0`.
- `code-review` (Spec y Standards en paralelo): limpio, 0 violaciones.
