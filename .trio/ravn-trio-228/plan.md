# Plan — Spec #228: ravn-trio: lease por ticket, HANDOFF.md vivo y pj-trio resume (Spec 2)

- **Spec:** `robert-flo/HR-CLAVIS#228`
- **Repo destino:** `robert-flo/ravn-trio`
- **Rama del spec:** `228-lease-handoff-resume` (base `main`, ya existente en `origin/228-lease-handoff-resume`)
- **Validación del repo:** `make check` (`shellcheck` + `tests/run.sh`)

## Orden de ejecución (DAG)

1. **`robert-flo/HR-CLAVIS#229`** — *ravn-trio: lease_expires_at en trio-status/trio.conf y HANDOFF.md vivo en prompts*
   - **Blocked by:** ninguno (arranca primero).
   - **Alcance:**
     1. Agregar `TRIO_LEASE_MINUTES=15` en `trio.conf.example` y su carga por capas en `trio_config`.
     2. Extender `trio-status` con `--lease` (y renovación automática al usar `--heartbeat`) para calcular y persistir `lease_expires_at` en ISO 8601 dentro de `.trio/<clave>/status.json`, conservando compatibilidad con todas las claves previas.
     3. Actualizar `trio-pm-prompt.md` y `trio-os-prompt.md` para que los agentes reescriban `.trio/<clave>/HANDOFF.md` (qué se hizo, qué falta y siguiente paso) y renueven su latido/lease en cada hito.
   - **Criterios de aceptación:**
     - `trio-status` con `--heartbeat` / `--lease` calcula y persiste `lease_expires_at` respetando `TRIO_LEASE_MINUTES`; verificado en `tests/test-status.sh` y `tests/test-config-capas.sh`.
     - `trio-pm-prompt.md` y `trio-os-prompt.md` incluyen la instrucción explícita de mantener `HANDOFF.md` y renovar el latido por hito.
     - `make check` (`shellcheck` + `tests/run.sh`) pasa en verde.
   - **Flujo:** worktree sobre `origin/228-lease-handoff-resume` → implementación → `/code-review` → sub-PR a `228-lease-handoff-resume` → merge del WK tras CI verde.

2. **`robert-flo/HR-CLAVIS#230`** — *ravn-trio: comando pj-trio resume con guarda de lease y relevo en herdr*
   - **Blocked by:** `#229` (se inicia tras mergear `#229` en `228-lease-handoff-resume`).
   - **Alcance:**
     - Registrar `resume` (alias `r`) en `runtime/commands.tsv`:
       `pj-trio resume <proyecto> <ticket> [--kind agy|cursor] [--role wk|rv|pm|os] [--repo <miembro>] [--ticket-repo <owner/repo>] [--force] [--dry-run] [--print-resume]`
     - Verificar que el worktree siga en disco y evaluar estado real (`fleet`) + lease: rechazar si está `vivo` sin `--force`; permitir relevo inmediato si está `sin-cuota`, `muerto` o con lease vencido (y siempre con `--force`).
     - Armar el prompt de relevo juntando `.trio/<clave>/HANDOFF.md` (si existe), `status.json`, `git status -sb`, `git log -n 5`, `git diff --stat` del worktree, estado del PR en `gh` y el `kickoff.md` original.
     - Reemplazar/reiniciar el panel del rol objetivo en el workspace existente de `herdr` (o crear workspace si no existe) con el nuevo `--kind`, preservando `--repo` (`cwd` del miembro + `--add-dir` del meta-repo), inyectar el prompt de relevo y actualizar `owner` + `lease_expires_at` en `status.json`.
   - **Criterios de aceptación:**
     - `pj-trio resume` rechaza cuando `estado_real == vivo` sin `--force` y releva cuando está `sin-cuota`, `muerto`, con lease vencido o con `--force`; verificado con stubs de `herdr` y `gh` en `tests/test-resume.sh`.
     - El prompt de relevo incluye `HANDOFF.md`, `status.json`, contexto `git` del worktree, estado del PR y `kickoff.md`, y `status.json` actualiza `owner` y `lease_expires_at`; verificado en `tests/test-resume.sh`.
     - `make check` (`shellcheck` + `tests/run.sh`) pasa completo en verde.
   - **Flujo:** worktree sobre `origin/228-lease-handoff-resume` (actualizado con `#229`) → implementación → `/code-review` → sub-PR a `228-lease-handoff-resume` → merge del WK tras CI verde.

3. **PR Final:**
   - Abrir PR final de `228-lease-handoff-resume` hacia `main` en `robert-flo/ravn-trio` (redactado con `/pr`, referenciando `robert-flo/HR-CLAVIS#228`, `#229`, `#230`, con `## Merge Danger` y pasos de validación manual).
   - Enviar a `rv-ravn-trio-228` para revisión y esperar `APRUEBO <sha>`.
