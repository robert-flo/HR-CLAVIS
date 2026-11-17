# Plan — robert-flo/ravn-trio#44

- **Ticket:** `robert-flo/ravn-trio#44` (`feat(watch): subcomando pj-trio watch y bump v0.1.11`)
- **Spec:** `#42` (sub-issue; `#43` / PR `#45` ya mergeado en `42-fleet-watch`)
- **Rama base:** `42-fleet-watch` (`origin/42-fleet-watch` @ `310f950`)
- **Worktree del WK:** `/z-home-repos/HR-CLAVIS/repos/ravn-trio/44-watch`
- **Rama de trabajo:** `issue/44-watch`

## Objetivo
Agregar `runtime/scripts/watch` y registrarlo en `runtime/commands.tsv` (alias `w`) siguiendo el estilo modular de `git-setup` (`AGENTS.md`), más el bump de `version` a `0.1.11` en `herdr-plugin.toml`.

## Contrato de `pj-trio watch`
`pj-trio watch <proyecto> [--snapshot <archivo>] [--timeout <seg>] [--json]`

1. Toma un snapshot de `<proyecto>/.trio/*/status.json` + estado `herdr agent get` de cada sub-PM activo (`en-curso`).
2. Si se pasa `--snapshot <archivo>` y ya hay diferencias respecto al snapshot previo, retorna de inmediato (`exit 0`) con el evento/diff y actualiza el snapshot.
3. Si no hay diferencias, observa `<proyecto>/.trio` con `inotifywait -e modify,create,moved_to -r -q -t <intervalo>` (30–60s por ciclo para chequear salud `herdr agent get` por si un sub-PM pasó a `blocked` o `unknown` sin escribir `status.json`).
4. Ante cualquier cambio de `paso`, `estado` (`hecho`, `needs-roberto`), o `blocked`/`unknown`:
   - Si es `blocked`, `unknown` o `needs-roberto`, captura últimas 25 líneas de `herdr agent read <pm-agente>`.
   - Imprime resumen (o JSON con `--json`) de qué cambió y sale `0`.
5. Si vence `--timeout` (default `1800` seg): sale `124`.

## Criterios de aceptación
1. `runtime/scripts/watch` implementado y registrado en `runtime/commands.tsv` (alias `w`).
2. `herdr-plugin.toml` actualizado a `version = "0.1.11"`.
3. `tests/test-watch.sh` probando diff contra `--snapshot`, detección de cambio de `paso`/`estado`, `blocked` y `timeout` con stubs de `herdr` e `inotifywait`.
4. `make lint` y `make test` en verde en el PR contra `42-fleet-watch`.

## Instrucciones para el WK
- Trabajar **solo** en `/z-home-repos/HR-CLAVIS/repos/ravn-trio/44-watch` (rama `issue/44-watch`).
- Leer `AGENTS.md`, `runtime/commands.tsv`, `runtime/scripts/fleet` y `tests/` del worktree para seguir exactamente las convenciones y helpers (`print_*`, `runtime/lib/*`).
- Correr `make lint` y `make test` antes de abrir el PR.
- Abrir el PR contra `42-fleet-watch` (`gh pr create -R robert-flo/ravn-trio --base '42-fleet-watch' ...`), usando la estructura de `/pr` (`## Summary`, `## Evidence`, `## Merge Danger`).
- Es un sub-issue: con CI verde y `code-review` limpio, mergear el PR a `42-fleet-watch` con `gh pr merge <pr> -R robert-flo/ravn-trio --squash --delete-branch --match-head-commit <sha-verde>`.
- Escribir `/z-home-repos/HR-CLAVIS/.trio/ravn-trio-44/wk.md` con URL del PR, head SHA, resultados de `make lint` / `make test` y SHA del merge.
