# Reporte de Implementación WK — robert-flo/ravn-trio#53

- **Ticket:** [robert-flo/ravn-trio#53](https://github.com/robert-flo/ravn-trio/issues/53) — `feat(trio): flag --oneshot (workspace de 1 panel os-*) y trio-os-prompt.md · v0.1.13`
- **PR:** [robert-flo/ravn-trio#54](https://github.com/robert-flo/ravn-trio/pull/54)
- **Base:** `52-modo-oneshot`
- **Head SHA:** `9f571842087aac54faebdac7801987914727568e`
- **Merge SHA:** `5a2c91da9043ee3120f1e41371f9a0c2885330fa`
- **Estado:** MERGED en `52-modo-oneshot` (squash & delete branch)

## Resumen de cambios implementados

1. **Plantilla de arranque para Oneshot (`trio-os-prompt.md`):**
   - Agregada en la raíz del repositorio y registrada para instalación en `$(DESTDIR)$(SHAREDIR)/trio-os-prompt.md` vía `Makefile`.
   - Instruye al agente autónomo `OS-<proyecto>` con: handoff único en `.trio/<clave>` del meta-repo, git config de Roberto sin trailers, worktree nuevo vía `ravn-git-bare-clone`, `Proof` real obligatoria, `code-review` limpio, PR redactado con `/pr`, evaluación estricta del Merge Gate (`Door: two-way` + `Blast Radius: Low` + CI verde + `code-review` limpio + `MERGEABLE`), y auto-merge con cierre del workspace en herdr (`herdr workspace close <id>`) o fallback a `needs-roberto` + `ready-to-merge`.

2. **Flag `--oneshot` en `start` y `kickoff` (`runtime/scripts/start`, `runtime/scripts/kickoff`):**
   - Soporte en parsing de argumentos (`--oneshot`) y en ayuda (`mostrar_uso`).
   - Workspace con 1 sola pestaña (`os`), panel renombrado a `OS-${ticket}`, agente `os-<miembro>-<ticket>` (`OS_KIND` / `OS-${proyecto}`), omitiendo pestañas y paneles `wk` y `rv`.
   - Localización y renderizado de `trio-os-prompt.md` (reemplazo de `{{AGENTE_OS}}`, `{{WORKSPACE_NOMBRE}}`, etc.).
   - Soporte idempotente en creación y reutilización de workspaces y tabs existentes en herdr.

3. **Configuración y variables (`runtime/lib/trio_config.sh`, `trio.conf.example`):**
   - Añadida variable `OS_KIND` con fallback automático a `${PM_KIND:-agy}`.
   - Búsqueda de `trio-os-prompt.md` en `${TRIO_SHARE_DIR}` y `${TRIO_ROOT}` o vía variable `PJ_TRIO_OS_PROMPT`.

4. **Supervisión de flota (`runtime/scripts/fleet` y `runtime/scripts/watch`):**
   - Si `pm-<miembro>-<ticket>` no está presente en herdr (o reporta estado "cerrado"), resuelven y consultan automáticamente el estado herdr de `os-<miembro>-<ticket>` (`nombre_agente os`).

5. **Catálogo y versión:**
   - Opciones actualizadas en `runtime/commands.tsv`.
   - Versión incrementada a `"0.1.13"` en `herdr-plugin.toml`.
   - Check de instalación de `trio-os-prompt.md` en `.github/workflows/ci.yml` y `tests/test-install.sh`.

## Pruebas verificadas

- `make lint`: shellcheck limpio sin advertencias ni errores en todo el árbol de scripts y tests.
- `make test`: suite de pruebas completa ejecutada con éxito (16/16 tests pasando):
  - `test-base-spec.sh`: OK
  - `test-clonar-proyecto.sh`: OK
  - `test-config-capas.sh`: OK
  - `test-dispatch-package.sh`: OK
  - `test-espera-agente.sh`: OK
  - `test-fleet.sh`: OK
  - `test-install.sh`: OK (verifica presencia de `trio-os-prompt.md`)
  - `test-oneshot.sh`: OK (prueba dry-run 1-panel, prompt kickoff, subcomando kickoff, y supervisión fleet de `os-*`)
  - `test-pj-trio-dry-run.sh`: OK
  - `test-plugin.sh`: OK
  - `test-pm-prompt-merge.sh`: OK (actualizado para validar versión 0.1.13)
  - `test-release.sh`: OK
  - `test-resolucion-repo.sh`: OK
  - `test-status.sh`: OK
  - `test-watch.sh`: OK
