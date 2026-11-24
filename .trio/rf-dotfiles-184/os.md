# OS Report — robert-flo/HR-CLAVIS#184

- **Issue:** https://github.com/robert-flo/HR-CLAVIS/issues/184 (`CLOSED`)
- **PR:** https://github.com/robert-flo/rf-dotfiles/pull/135 (`MERGED`)
- **Head SHA:** `4ba80f4bca74e902bda8c9272418ed32f443922d`
- **Merge Commit SHA:** `d25fa4ef3a1454d58927c967042147565228bf3e`
- **Merge Danger:** `two-way / Low`

## Cambios aplicados
- `scripts/spicetify-permissions.hook`: hook `PostTransaction` de `pacman` para restaurar permisos `a+wr` en `/opt/spotify` y `/opt/spotify/Apps` tras instalar/actualizar `spotify`.
- `run_onchange_after_02-install-system-tools.sh.tmpl`: agregado `# spicetify-permissions.hook hash: {{ include "scripts/spicetify-permissions.hook" | sha256sum }}` e instalación con `$SUDO_CMD install -Dm644 "{{ .chezmoi.sourceDir }}/scripts/spicetify-permissions.hook" /etc/pacman.d/hooks/spicetify-permissions.hook`.

## Evidencia de pruebas
- `chezmoi execute-template` + `bash -n` + `shellcheck -s bash -`: limpio (`syntax+shellcheck OK`).
- `chezmoi apply --source=... --include=scripts --force`: instaló `/etc/pacman.d/hooks/spicetify-permissions.hook` con permisos `644 root:root` y contenido exacto.
- `code-review` (Standards + Spec): limpio, sin violaciones ni desviaciones del spec.
