# Reporte OS — `robert-flo/HR-CLAVIS#59` (`rf-dotfiles`)

- **PR:** https://github.com/robert-flo/rf-dotfiles/pull/75
- **Head SHA:** `28847c6b8cf62e64e8754dc0a97cf578e8d34247`
- **Merge SHA:** `eba82e5b8096812e31a1bf45613359eb4319c3df`
- **Merge Danger:** `two-way / Low`
- **Issue:** https://github.com/robert-flo/HR-CLAVIS/issues/59 (`CLOSED`, `labels: []`)

## Qué cambió
En `dot_config/fastfetch/config.jsonc.tmpl` (solo rama `{{- else -}}`, `niri-upstream` intacta):
- `logo.height`: `18` → `13` y `logo.padding`: `{ "top": 1, "left": 1, "right": 2 }`, manteniendo `type: "kitty"` y `source` con `{{ .chezmoi.homeDir }}`.
- Claves (`key`) compactadas de 15 a 11 caracteres y bordes horizontales de las 3 cajas acortados de 60 a 50 columnas.
- `weather`: agregado `"outputFormat": "%t+-+%C"` manteniendo `"location": "Ilopango,El_Salvador"` y `"timeout": 5000`; agregado debajo módulo `custom` `Location` (`" 󰍎 Location"` → `"Ilopango, El Salvador"`).

## Evidencia de pruebas
- `chezmoi execute-template` para `niri-upstream` vs `.upstream/fastfetch/config.jsonc`: `IDENTICAL (0 bytes diff)`.
- Render para `niri-test`: JSON válido (`jq .logo` OK), cero rutas `/home/<user>` hardcodeadas en el template.
- `sudo -u niri-test -H chezmoi apply` + `sudo -u niri-test -H fastfetch --logo none`: `MAX_COLS=50` (0 desbordes; `CPU`=50, `Display`=41, `Storage`=43, `Weather`=40 sin ubicación repetida, `Location`=35).
- `chezmoi diff` + `chezmoi apply` aplicado también en `niri` (con respaldo previo y registro en `/z-home-repos/CAMBIOS.md`) y `/z/rf-dotfiles` sincronizado a `eba82e5`.
- `code-review`: Standards clean (0 violaciones) y Spec PASS (0 hallazgos).
