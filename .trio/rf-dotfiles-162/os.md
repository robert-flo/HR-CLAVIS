# Reporte Oneshot (OS) - rf-dotfiles #162

- **Ticket:** [robert-flo/HR-CLAVIS#162](https://github.com/robert-flo/HR-CLAVIS/issues/162)
- **PR:** [robert-flo/rf-dotfiles#121](https://github.com/robert-flo/rf-dotfiles/pull/121)
- **Head SHA:** `f6a8c0875b5256cb29295c0d5cbf154d03a018c6`
- **Merge SHA:** `1deab00dd775a7b1030dfa25dfd38c3083eddc9b`
- **Merge Danger:** `Door: two-way` / `Blast Radius: Low`
- **Estado:** Hecho (auto-mergeado exitosamente bajo Merge Gate)

## Cambios realizados
1. Agregado `dot_local/bin/executable_clavis-meeting-mode.sh` con permisos ejecutables (toggle de audio y micrófono vía `wpctl` con notificaciones y archivo de estado en `${XDG_RUNTIME_DIR}`).
2. Agregado el binding `Mod+Shift+M allow-when-locked=true hotkey-overlay-title="Modo Videollamada: Toggle Audio Total" { spawn-sh "clavis-meeting-mode.sh"; }` en `.personal/niri/config.kdl` dentro de la sección de control de volumen/audio.

## Evidencia de pruebas
- `shellcheck dot_local/bin/executable_clavis-meeting-mode.sh`: Limpio, código 0.
- `bash -n dot_local/bin/executable_clavis-meeting-mode.sh`: Sintaxis válida, código 0.
- `niri validate -c ~/.config/niri/config.kdl`: `INFO niri: config is valid`, código 0.
- Prueba dinámica en vivo del script: Ejecución limpia del ciclo mute total -> unmute total, sin fugas ni errores de ejecución.
- `chezmoi diff`: Configuración sincronizada y limpia contra los archivos de usuario.
- CI: Sin checks fallando o pendientes; Mergeable limpio.
