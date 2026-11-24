# Reporte de Implementación del WK — robert-flo/HR-CLAVIS#145

- **Spec:** [robert-flo/HR-CLAVIS#145](https://github.com/robert-flo/HR-CLAVIS/issues/145) — `spec: migrar utilidades de pj-domina-rf-tools hacia rf-dotfiles`
- **Repo destino:** `robert-flo/rf-dotfiles`
- **Rama base:** `master`
- **Rama del spec:** `145-migrar-tools-a-rf-dotfiles`
- **PR Final:** [robert-flo/rf-dotfiles#113](https://github.com/robert-flo/rf-dotfiles/pull/113)
- **Head SHA:** `219bbefa6f23ea1092007da56c3fb523213a66dc`

---

## 1. Sub-issues Implementados y Merges Parciales

| Sub-issue | Título | Sub-PR | Estado |
|---|---|---|---|
| [#146](https://github.com/robert-flo/HR-CLAVIS/issues/146) | `gup, wgup y atajo agent: migrar a dot_local/bin en rf-dotfiles` | [#110](https://github.com/robert-flo/rf-dotfiles/pull/110) | Squash merge en `145-migrar-tools-a-rf-dotfiles` (`92d8183`) |
| [#147](https://github.com/robert-flo/HR-CLAVIS/issues/147) | `cursor-statusline: migrar scripts a dot_config/cursor en rf-dotfiles` | [#111](https://github.com/robert-flo/rf-dotfiles/pull/111) | Squash merge en `145-migrar-tools-a-rf-dotfiles` (`c63ece7`) |
| [#148](https://github.com/robert-flo/HR-CLAVIS/issues/148) | `brief: migrar scripts, librería, units systemd y hook de build para brief-table Go` | [#112](https://github.com/robert-flo/rf-dotfiles/pull/112) | Squash merge en `145-migrar-tools-a-rf-dotfiles` (`219bbef`) |
| [#149](https://github.com/robert-flo/HR-CLAVIS/issues/149) | `integracion y validacion: chezmoi apply y verificación de herramientas en sistema` | N/A (Validación) | Verificado en vivo y cerrado |

---

## 2. Archivos Tocados

```text
dot_config/cursor/executable_statusline-usage.sh
dot_config/cursor/executable_statusline.sh
dot_config/systemd/user/brief.service
dot_config/systemd/user/brief.timer
dot_local/bin/executable_brief
dot_local/bin/executable_brief-session
dot_local/bin/executable_brief-trend
dot_local/bin/executable_gup
dot_local/bin/symlink_agent
dot_local/bin/symlink_wgup
dot_local/lib/brief/brief-common.sh
run_onchange_after_install-brief-table.sh.tmpl
scripts/brief-table/go.mod
scripts/brief-table/go.sum
scripts/brief-table/main.go
```

Total: 15 archivos añadidos/modificados (+2324 líneas), cero co-authors o trailers de IA.

---

## 3. Pruebas y Evidencias Reales

1. **Sintaxis y Linter:**
   - `bash -n` limpio en todos los scripts de shell y plantillas Chezmoi.
   - `shellcheck` limpio al 100% (incluyendo corrección de SC1087 en `${E}[...]`).
   - `systemd-analyze verify` ejecutado sobre `brief.service` y `brief.timer` sin errores de configuración.

2. **Compilación Go:**
   - `go build` en `scripts/brief-table` exitoso.
   - Hook Chezmoi `run_onchange_after_install-brief-table.sh.tmpl` compila automáticamente al detectar cambios de hash en `scripts/brief-table/main.go`.

3. **Chezmoi Diff y Apply:**
   - `chezmoi diff --source=/z-home-repos/HR-CLAVIS/repos/rf-dotfiles/149-integracion-chezmoi` ejecutado sin colisiones ni rutas no deseadas.
   - `chezmoi apply --source=/z-home-repos/HR-CLAVIS/repos/rf-dotfiles/149-integracion-chezmoi --force` completado con éxito (código de salida 0).

4. **Verificación en Vivo en la Estación:**
   - `/home/niri/.local/bin/gup --help` y `/home/niri/.local/bin/wgup --help`: funcionales; allowlist verifica `~/.config/clavis/gup.allow` y `~/.config/robertos/gup.allow`.
   - `/home/niri/.local/bin/agent`: symlink resolviendo a `cursor-agent`.
   - `/home/niri/.local/bin/brief --help` y `/home/niri/.local/bin/brief-trend --help`: funcionales con resolución dinámica de la librería `brief-common.sh`.
   - `/home/niri/.local/bin/brief-table -h`: binario Go disponible y funcional.
   - `/home/niri/.config/cursor/statusline.sh`: ejecutado con JSON sintético por stdin; renderiza sesión y cuotas de uso correctamente.
   - `systemctl --user status brief.timer`: unidad cargada correctamente por el systemd user daemon.

---

## 4. Estado de Ramas y Worktrees

- Worktrees efímeros (`146-gup-wgup-agent`, `147-cursor-statusline`, `148-brief-tools`, `149-integracion-chezmoi`) removidos limpiamente.
- Ramas remotas temporales eliminadas tras cada squash merge.
- PR Final [robert-flo/rf-dotfiles#113](https://github.com/robert-flo/rf-dotfiles/pull/113) listo para revisión del RV (`master` <- `145-migrar-tools-a-rf-dotfiles`).
