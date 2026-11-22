# Entrega WK — robert-flo/HR-CLAVIS#119

- **PR:** https://github.com/robert-flo/ravn-trio/pull/62
- **Base:** `116-trio-por-spec-y-herdr-resiliente`
- **Head SHA:** `a8ab6555062c86b87b4676c5c4bd83e62653a635`
- **Merge SHA:** `1a2816d393346f3b7308a0b3c14077c4c78ff68f` (squash merge a `116-trio-por-spec-y-herdr-resiliente`, rama remota `issue/119-pm-name-herdr` eliminada)

## Cambios realizados

- `runtime/lib/trio_herdr.sh`: `trio_construir_args_agente` arranca `agy` sin `-i` para que `herdr` confirme `interactive_ready: true` y registre el `name` del agente (`pm-*` / `os-*`); `enviar_kickoff` envía el arranque de una línea (`Lee y segui al pie de la letra las instrucciones de arranque del archivo .../kickoff.md`) vía `herdr agent prompt`, manteniendo fallback por `pane_id` y aviso manual si `herdr` no confirma.
- `runtime/lib/trio_agy.sh`: `esperar_agente_listo` consulta `.result.agent.agent_status` además de `.status` y acepta `fallback_pane` cuando el agente no tiene `name`.
- `runtime/lib/trio_project.sh`, `runtime/scripts/watch`, `runtime/scripts/fleet`: `trio_resolver_agente_herdr` busca primero por `name` (`pm-*` / `os-*`) y hace fallback por `workspace_id` / `pane_id` (`herdr workspace list` + `herdr pane list --workspace` / `herdr agent list`) antes de reportar `unknown` o `cerrado`.
- `tests/test-pm-name-herdr.sh` y ajustes en `tests/test-pj-trio-dry-run.sh`, `tests/test-oneshot.sh`, `tests/test-resolucion-repo.sh`.

## Pruebas

- `make lint` (`shellcheck -x pj-trio trio-status runtime/scripts/* runtime/lib/*.sh runtime/helper/*.sh runtime/plugin/* runtime/completion/*.bash tests/*.sh`): **0 errores**.
- `make test`: **All tests passed successfully!** (18 suites en verde, incluyendo `tests/test-pm-name-herdr.sh` con mocks de `herdr` y `agy`, `test-watch.sh` y `test-fleet.sh`).
- `code-review` (dos ejes: Standards + Spec): limpio tras corregir el bucle de `herdr agent get` y el fallback de `jq` para `""` en `.name`.
