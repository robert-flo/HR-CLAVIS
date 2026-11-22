# OS — robert-flo/HR-CLAVIS#130

- **Ticket:** https://github.com/robert-flo/HR-CLAVIS/issues/130
- **PR:** https://github.com/robert-flo/rf-dotfiles/pull/102
- **Head SHA:** `4bf9423768a670c538b1e2b1fabbace4e4a68e19`
- **Merge SHA:** `dc24bf909829e97c5df190b0506ab19ae2292d6f`
- **Merge Danger:** `two-way / Low`

## Cambio

En `dot_config/herdr/config.toml` de `robert-flo/rf-dotfiles`:
- `ctrl+shift+up` → `alt+shift+up` para `"$HOME/.local/bin/herdr-move-workspace" up`
- `ctrl+shift+down` → `alt+shift+down` para `"$HOME/.local/bin/herdr-move-workspace" down`

## Evidencia de pruebas y revisión

- `python3 -c "import tomllib; tomllib.load(open('dot_config/herdr/config.toml','rb'))"` → `TOML OK`
- `chezmoi diff --source=/z/repos/rf-dotfiles/130-herdr-move-workspace -- ~/.config/herdr/config.toml` → sin diferencias en los atajos `alt+shift+up`/`down` ni `sidebar_collapsed_mode`
- `herdr server reload-config` → `{"id":"cli:server:reload-config","result":{"diagnostics":[],"status":"applied","type":"config_reload"}}`
- `code-review` (Standards y Spec) → limpio, 0 hallazgos
- `gh pr checks 102 -R robert-flo/rf-dotfiles --watch` → `MERGEABLE`, sin checks fallando ni pendientes
