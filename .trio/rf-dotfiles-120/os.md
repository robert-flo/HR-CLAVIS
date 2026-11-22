# Reporte OS — `robert-flo/HR-CLAVIS#120`

- **Ticket:** [robert-flo/HR-CLAVIS#120](https://github.com/robert-flo/HR-CLAVIS/issues/120)
- **PR:** [robert-flo/rf-dotfiles#101](https://github.com/robert-flo/rf-dotfiles/pull/101)
- **Head SHA:** `4d31694a899b113cec1483f0f9dca67ec51b5b7e`
- **Merge Commit SHA:** `c2274c40a619c338fa4117c770b4a3bef7583603`
- **Merge Danger:** `two-way / Low`

## Cambio realizado

En `dot_config/herdr/config.toml` dentro de `[ui]`, se agregó `sidebar_collapsed_mode = "hidden"` manteniendo intactos `sidebar_start_collapsed = true` y `hide_tab_bar_when_single_tab = true`.

## Evidencia de pruebas reales

- `chezmoi diff --source=/z/repos/rf-dotfiles/120-herdr-sidebar-hidden -- ~/.config/herdr/config.toml`: mostró únicamente `+sidebar_collapsed_mode = "hidden"` en `[ui]` respecto a la rama `master` de `rf-dotfiles`.
- `chezmoi apply --source=/z/repos/rf-dotfiles/120-herdr-sidebar-hidden --destination=/zt/test-120-dest --force -- /zt/test-120-dest/.config/herdr/config.toml` + `XDG_CONFIG_HOME=/zt/test-120-dest/.config herdr config check`: `config: ok`.
- `python3` `tomllib.load` sobre `dot_config/herdr/config.toml`: `TOML OK`.
- `code-review`: Standards (0 violaciones, 0 smells) y Spec (PASS).
