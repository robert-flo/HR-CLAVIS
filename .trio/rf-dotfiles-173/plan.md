# Plan — Spec #173: Integrar configuración de Zed en rf-dotfiles (Rollo + rf-dotfiles)

- **Spec:** `robert-flo/HR-CLAVIS#173`
- **Repo destino:** `robert-flo/rf-dotfiles` (rama base: `master`)
- **Rama del spec:** `173-zed-config-integration`

## Orden de sub-issues

1. **`robert-flo/HR-CLAVIS#174`** (`blocked-by: none`) — `173 - Fusionar configuración de Zed (Rollo + rf-dotfiles) en dot_config/zed`
   - **Objetivo:**
     - Fusionar `/z-home-repos/HR-CLAVIS/repos/Rollo/master/Configs/.config/zed/settings.json` en `dot_config/zed/settings.json`, ganando `Rollo` en conflictos (`vim_mode: true`, `Tokyo Night`, `Maple Mono NF`, `tab_bar.show: true`, `dock: "right"`, `lsp`, `languages`, `file_types`, `file_scan_exclusions`, `autosave`, `git.inline_blame`, `code_lens`, `auto_signature_help`, `centered_layout`, `terminal.env.EDITOR`) y preservando propiedades exclusivas de `rf-dotfiles` que no chocan (`auto_update: false`, `telemetry` off, `window_decorations: "client"`, `title_bar` minimalista, `debugger.button: false`, `agent` con `always_allow_tool_actions: false` y `default_profile: "write"`, `restore_on_startup`, `preview_tabs.enabled: false`, `edit_predictions_disabled_in`, `unnecessary_code_fade`, `calls`, `audio`, `current_line_highlight`, `hide_mouse`, `multi_cursor_modifier`, `agent_ui_font_size`, `agent_buffer_font_size`, `play_sound_when_agent_done: false`).
     - Agregar `dot_config/zed/keymap.json` desde `/z-home-repos/HR-CLAVIS/repos/Rollo/master/Configs/.config/zed/keymap.json`.
     - Agregar `dot_config/zed/tasks.json` desde `/z-home-repos/HR-CLAVIS/repos/Rollo/master/Configs/.config/zed/tasks.json`.
   - **Criterios de aceptación y pruebas:**
     - Sintaxis JSONC válida en `settings.json`, `keymap.json` y `tasks.json`.
     - `chezmoi diff` (y `chezmoi apply` en testbench `niri-test` / `omarchy-test` si hay acceso, o `chezmoi apply --dry-run` / validación de plantillas) sin errores, sin rutas hardcodeadas ni contaminación cruzada.
     - `code-review` limpio y PR hacia la rama del spec `173-zed-config-integration` con salida de pruebas en el body (`/pr`).

## PR Final
- Rama `173-zed-config-integration` → `robert-flo/rf-dotfiles:master` redactado con `/pr` (referenciando `robert-flo/HR-CLAVIS#173`), con evidencia de validación y `## Merge Danger`.
