# Plan — robert-flo/ravn-trio#53

- **Ticket:** [robert-flo/ravn-trio#53](https://github.com/robert-flo/ravn-trio/issues/53) — `feat(trio): flag --oneshot (workspace de 1 panel os-*) y trio-os-prompt.md · v0.1.13`
- **Repo:** `robert-flo/ravn-trio` (rama base: `origin/52-modo-oneshot` / `52-modo-oneshot`)
- **Worktree:** `/z/repos/ravn-trio/53-flag-oneshot-os`
- **Rama:** `issue/53-flag-oneshot-os`

## Objetivo

Agregar el flag `--oneshot` a `pj-trio start` y `pj-trio kickoff`, permitiendo crear un workspace visible en `herdr` con 1 solo panel (`os`, agente `OS-<proyecto>`, nombre `os-<miembro>-<ticket>`), renderizando la plantilla de arranque `trio-os-prompt.md` y garantizando que `fleet` y `watch` reconozcan y supervisen al agente `os-*` en tickets de 1 panel.

## Cambios requeridos

1. **Plantilla `trio-os-prompt.md`:**
   - Crear `trio-os-prompt.md` en la raíz del repo (junto a `trio-pm-prompt.md`), conteniendo las instrucciones de arranque para el agente `OS-<proyecto>`.
   - Incluye: handoff en `.trio/<clave>`, `trio-status`, identidad git de Roberto sin trailers, worktree nuevo vía `ravn-git-bare-clone`, `Proof` real obligatoria, `code-review`, apertura de PR con `/pr` (`## Summary`, `## Evidence`, `## Merge Danger`), evaluación del Merge Gate (`Door: two-way` + `Blast Radius: Low` + CI verde + `code-review` limpio + `MERGEABLE`), auto-merge si califica y cierre de workspace con `herdr workspace close`, o parada con `ready-to-merge` y `needs-roberto` si no califica.

2. **`runtime/scripts/start` y `runtime/scripts/kickoff`:**
   - Soportar el flag `--oneshot` en parsing de argumentos y ayuda (`--help`).
   - Cuando `--oneshot` está activo:
     - En `start`: crear el workspace con 1 sola pestaña/panel para el rol `os` (`panel_os`, agente `OS-<proyecto>`, nombre herdr `os-<miembro>-<ticket>`), omitiendo `wk` y `rv`.
     - Ubicar y renderizar `trio-os-prompt.md` en vez de `trio-pm-prompt.md`.
     - Iniciar al agente con `OS_KIND` / `PM_KIND` (o agy con prompt renderizado).
     - En `kickoff`: aceptar `--oneshot` y renderizar `trio-os-prompt.md`.

3. **`runtime/lib/trio_config.sh` y helpers:**
   - Agregar resolución de `trio-os-prompt.md` (o variable `PJ_TRIO_OS_PROMPT` con fallback a `${TRIO_SHARE_DIR}/trio-os-prompt.md` y `${TRIO_ROOT}/trio-os-prompt.md`).

4. **`runtime/scripts/fleet` y `runtime/scripts/watch`:**
   - Compatibilidad con workspaces `--oneshot`: al consultar el estado del agente herdr para una clave, si `pm-<miembro>-<ticket>` no está presente en herdr (o da `cerrado`/no encontrado), resolver y consultar `os-<miembro>-<ticket>` (`nombre_agente os`).
   - Asegurar que `fleet` y `watch` reporten el status herdr de `os-*` correctamente.

5. **`Makefile` y Layout de Instalación:**
   - Instalar `trio-os-prompt.md` en `$(DESTDIR)$(SHAREDIR)/trio-os-prompt.md`.
   - Asegurar que los checks de CI/instalación verifiquen la presencia de `trio-os-prompt.md`.

6. **`herdr-plugin.toml`:**
   - Incrementar la versión a `"0.1.13"`.

7. **Tests:**
   - Crear/actualizar pruebas en `tests/` para verificar:
     - `pj-trio start <proyecto> <ticket> --oneshot [--dry-run] [--print-kickoff]` crea 1 solo panel `os-*` y usa `trio-os-prompt.md`.
     - `trio-os-prompt.md` se instala correctamente (`test-install.sh`).
     - `pj-trio fleet` y `pj-trio watch` resuelven `os-<miembro>-<ticket>`.
   - Verificar `make lint` y `make test` en verde.

8. **Entrega y Merge del Sub-issue:**
   - Abrir PR contra `52-modo-oneshot` (`--base 52-modo-oneshot`).
   - WK corre `code-review` y verifica CI verde.
   - Siendo sub-issue con base `52-modo-oneshot`, WK mergea su propio PR con `--squash --delete-branch --match-head-commit <sha>` y anota el SHA del merge en `wk.md`.
