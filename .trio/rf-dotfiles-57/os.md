# Reporte OS — `robert-flo/HR-CLAVIS#57` (`rf-dotfiles`)

- **Issue:** https://github.com/robert-flo/HR-CLAVIS/issues/57 (CLOSED, sin etiquetas de triage)
- **PR:** https://github.com/robert-flo/rf-dotfiles/pull/73
- **Head SHA:** `455394bdb0bf90b968ae3df553f6a05a13b5611a`
- **Merge SHA:** `3d378ca153410607d4d272cd458bef3fde7e8435`
- **Merge Danger:** `two-way / Low` (auto-merge aprobado por Merge Gate)

## Cambios realizados
1. `.chezmoidata/packages.json`: agregados `ttf-cascadia-code-nerd` (`official`) y `maplemono-nf-unhinted` (`aur`).
2. `dot_config/kitty/robert.conf.tmpl` y `dot_config/ghostty/config`: activada `Maple Mono NF` por defecto y dejadas comentadas `CaskaydiaCove Nerd Font Mono` y las demás fuentes solicitadas.
3. `dot_config/kitty/theme.conf.tmpl`: versionado tema Catppuccin Mocha con el mismo guard de usuarios de `kitty.conf.tmpl`; mantenido `# include theme.conf` comentado en `.upstream/kitty/kitty.conf`.
4. `dot_config/niri/terminals-blur.kdl`, `.personal/niri/config.kdl` y `.chezmoiignore.tmpl`: regla `window-rule` de blur nativo Wayland para `kitty` y `com.mitchellh.ghostty`, excluida en `clavis-upstream`; `background_opacity 0.85` en `robert.conf.tmpl` y removido `dim_opacity 0.80`.
5. Limpieza de duplicados en `dot_config/kitty/robert.conf.tmpl`, eliminado `dot_config/kitty/themes/dark-test.conf`, y eliminados archivos residuales locales `/home/niri/.config/kitty/{kitty.conf.bak-20261121-1428,kitty.md}`.

## Evidencia de pruebas
- `pacman -Q ttf-cascadia-code-nerd maplemono-nf-unhinted`: `3.5.1-2` y `7.9-1` instalados vía `chezmoi apply`.
- `fc-list | grep -iE "Maple Mono NF|CaskaydiaCove Nerd Font Mono"`: ambas familias detectadas con nombres exactos.
- `kitty +runpy` (`load_config("/home/niri/.config/kitty/kitty.conf")`): `font_family: Maple Mono NF | background_opacity: 0.85 | dim_opacity: 0.4 | allow_remote_control: socket-only | listen_on: unix:/tmp/kitty | enable_audio_bell: False`.
- `ghostty +validate-config`: exit 0 sin errores.
- `niri validate` (`niri`) y `sudo -u niri-test niri validate -c /home/niri-test/.config/niri/terminals-blur.kdl`: `INFO niri: config is valid`.
- `chezmoi status ~/.config/kitty ~/.config/ghostty ~/.config/niri`: limpio.
- `git -C /z/rf-dotfiles pull --ff-only origin master`: sincronizado a `3d378ca`.
- `code-review` (Standards + Spec): limpio.
