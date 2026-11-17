# Plan — robert-flo/ravn-trio#43

- **Ticket:** `robert-flo/ravn-trio#43` — `feat(fleet): subcomando pj-trio fleet para estado consolidado de la flota`
- **Spec padre:** `#42` (rama base: `42-fleet-watch`)
- **Worktree:** `/z-home-repos/HR-CLAVIS/repos/ravn-trio/43-fleet`
- **Rama:** `issue/43-fleet` (base: `origin/42-fleet-watch`)
- **PR contra:** `42-fleet-watch`

## Objetivo
Agregar `runtime/scripts/fleet` y registrarlo en `runtime/commands.tsv` (alias `fl`), siguiendo el estilo modular de `git-setup` (`AGENTS.md`, helpers en `runtime/lib/`, `lib/trio_project.sh`, `print_*`).

## Contrato
`pj-trio fleet <proyecto> [--activos] [--json]`
- Resuelve `<proyecto>` igual que `start`/`kickoff` (usando `lib/trio_project.sh`).
- Lee `<proyecto>/.trio/*/status.json`.
- Cruza con `herdr workspace list` y `herdr agent get <pm-agente>` (si `herdr` está disponible; si no, degrada limpio marcando `herdr=unavailable` o `cerrado`).
- Con `--activos`: filtra solo los que tienen `estado == "en-curso"` o `needs-roberto` o workspace abierto.
- Con `--json`: imprime array JSON con `clave`, `ticket`, `estado`, `paso`, `pm_herdr_status`, `workspace_id`, `pr`, `head`, `merge_danger`, `timestamp`.
- Sin `--json`: imprime tabla legible con `print_*` / formato limpio.

## Criterios de aceptación
1. `runtime/scripts/fleet` implementado y registrado en `runtime/commands.tsv` (alias `fl`).
2. `tests/test-fleet.sh` probando salida tabla y `--json`, `--activos` y degradación sin `herdr` usando stubs.
3. `make lint` y `make test` en verde en el PR contra `42-fleet-watch`.

## Notas para el WK
- Es un sub-issue de `#42`: abrir el PR contra `42-fleet-watch` (no `main`), sin pasar por el RV.
- Con CI verde y `code-review` limpio, mergear el PR a `42-fleet-watch` con `gh pr merge <pr> -R robert-flo/ravn-trio --squash --delete-branch --match-head-commit <sha-verde>` y anotar el SHA del merge en `/z-home-repos/HR-CLAVIS/.trio/ravn-trio-43/wk.md`.
- Identidad git: `Roberto Flores <25asab015@ujmd.edu.sv>`, sin trailers `Co-authored-by` ni atribución a agentes.
