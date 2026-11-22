# WK — Ticket robert-flo/HR-CLAVIS#138

- **PR:** https://github.com/robert-flo/ravn-trio/pull/69
- **Head SHA:** `4b791803d28788d12850647404d14ee1fd1d5177`
- **Rama:** `issue/138-interactive-ready-kickoff-v2` (base `main`)
- **Worktree:** `/z/repos/ravn-trio/138-interactive-ready-kickoff`
- **Versión (`herdr-plugin.toml`):** `0.1.19`

## Cambios realizados

1. `runtime/lib/trio_agy.sh` (`esperar_agente_listo`):
   - Exige `agent_status == "idle"`, `interactive_ready == true` en `herdr agent get` y presencia del prompt `>` en `herdr agent read` (sin diálogo de confianza).
2. `runtime/lib/trio_herdr.sh` (`_kickoff_aceptado`, `enviar_kickoff`):
   - Tras `trio_herdr agent prompt`, verifica (`TRIO_KICKOFF_VERIFY_SECS`, default `2`s) que el agente salió de `idle` o muestra `Generating...` en `herdr agent read`.
   - Si sigue `idle`, limpia la línea con `trio_herdr agent send-keys <target> ctrl+u` y reintenta hasta `TRIO_KICKOFF_RETRIES` (default `2` veces).
   - Si agota los reintentos, marca `status.json` como `blocked` vía `trio-status` con `--cola-de` y `--error`.
3. `runtime/scripts/{status,watch,fleet}`:
   - Soportan el estado `blocked` en `status.json` para que `pj-trio watch` despierte de inmediato y `pj-trio fleet --activos` lo incluya.
4. `herdr-plugin.toml` y `tests/test-pm-prompt-merge.sh`:
   - Bump de versión a `0.1.19`.
5. Tests CLI:
   - `tests/test-espera-agente.sh`: casos negativos para `interactive_ready=false` y ausencia de prompt `>`.
   - `tests/test-pm-name-herdr.sh`: mock actualizado para reflejar transición a `working` tras `agent prompt` e `interactive_ready: true` en fallback por `pane_id`.
   - `tests/test-kickoff-retry-blocked.sh`: verifica reintento con `ctrl+u` y recuperación (`Generating...`), agotamiento de reintentos con marcado `blocked` en `status.json`, y despertar inmediato de `pj-trio watch`.

## Pruebas

- `make lint` (`shellcheck -x ...`): OK (0 warnings/errors).
- `make test` (`bash tests/run.sh`): 22/22 suites en verde (`All tests passed successfully!`).
