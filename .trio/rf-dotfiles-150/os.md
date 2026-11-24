# Reporte Final OS: Ticket #150 (rf-dotfiles)

- **Issue:** [robert-flo/HR-CLAVIS#150](https://github.com/robert-flo/HR-CLAVIS/issues/150)
- **PR:** [robert-flo/rf-dotfiles#114](https://github.com/robert-flo/rf-dotfiles/pull/114)
- **Merge Commit:** `2f1b9b4118c55144035f827e110708b9fc76e6e6`
- **Merge Danger:** `**Door:** two-way` / `**Blast Radius:** Low`

---

## Resumen del Cambio
Se creó el hook de Chezmoi `run_onchange_after_configure-cursor-statusline.sh.tmpl` en `rf-dotfiles`:
1. Incluye la directiva de hash de `dot_config/cursor/executable_statusline.sh` para activarse automáticamente ante cambios.
2. Inyecta de forma idempotente con `jq` el bloque `statusLine` apuntando a `{{ .chezmoi.homeDir }}/.config/cursor/statusline.sh` en `{{ .chezmoi.homeDir }}/.config/cursor/cli-config.json`.
3. Inicializa `cli-config.json` con `{}` si no existía o no era JSON válido.
4. Si `~/.cursor` existe y no cuenta con `cli-config.json`, genera un symlink relativo/defensivo hacia `../.config/cursor/cli-config.json`.

---

## Evidencia de Pruebas (Proof)
1. **Sintaxis y Linter:**
   - `bash -n` y `shellcheck` limpios sobre la plantilla.
2. **Validación en Testbench (`niri-test`):**
   - Ejecutado `chezmoi apply --source=... --force` en usuario de prueba `niri-test`.
   - Se comprobó la generación correcta de `/home/niri-test/.config/cursor/cli-config.json` con el comando exacto.
   - Se probó la creación del symlink en `/home/niri-test/.cursor/cli-config.json`.
3. **Aplicación en Producción y Verificación Local:**
   - `chezmoi apply --source=... --include=scripts` ejecutado exitosamente en `niri`.
   - `~/.config/cursor/cli-config.json` contiene `statusLine`:
     ```json
     "statusLine": {
       "type": "command",
       "command": "/home/niri/.config/cursor/statusline.sh"
     }
     ```
   - `~/.cursor/cli-config.json` apunta defensivamente a `~/.config/cursor/cli-config.json`.
   - Idempotencia validada en ejecuciones consecutivas (0 cambios adicionales).
4. **Sincronización:**
   - Clon central `/z/rf-dotfiles` actualizado con `git pull --ff-only origin master`.

---

## Merge Gate y Cierre
- Criterios cumplidos: CI sin fallos, rama mergeable, pruebas reales aprobadas, riesgo two-way / Low.
- Auto-merge ejecutado con squash hacia `origin/master`.
- Rama auxiliar `issue/150-cursor-statusline` eliminada en remoto y worktree local listo para remoción.
