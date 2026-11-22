# Plan: robert-flo/HR-CLAVIS#89 — Soportar `--kind <agy|cursor>` en `pj-trio`

## Ticket
- Issue: https://github.com/robert-flo/HR-CLAVIS/issues/89 (parent `#85`)
- Repo de código: `robert-flo/ravn-trio`
- Worktree: `/z/repos/ravn-trio/89-kind-cursor`
- Rama: `issue/89-kind-cursor`
- Base: `origin/HEAD` (`faf3b2c`, `main`, v0.1.15)
- PR destino: `main` de `robert-flo/ravn-trio`

## Qué construir
Añadir soporte para `--kind <agy|cursor>` en `pj-trio` (`start` y `--oneshot`, manteniendo `agy` como default si se omite el flag y sin introducir `PJ_TRIO_KIND`).

Cuando `--kind cursor`:
1. Validar disponibilidad de `cursor-agent` (en `PATH` o vía `mise x cursor-agent`) antes de abrir panes, fallando rápido si falta.
2. Lanzar cada pane con:
   `herdr agent start <nombre> --kind cursor --pane <id> -- --yolo --trust --workspace <repo-ticket> --add-dir <home-repo>`
3. En `pm` y `os` (`--oneshot`), pasar inline tras `--` la mención por ruta absoluta `@<home-repo>/.cursor/rules/{pm-trio,os}-<slug>.mdc` seguida de la línea de kickoff estándar.
4. En `wk` y `rv`, iniciar sin prompt posicional (igual que `agy` sin `-i`) y actualizar `trio-pm-prompt.md` para que el PM efímero anteponga `@<home-repo>/.cursor/rules/{wk,rv}-<slug>.mdc` al enviar `herdr agent prompt`.

## Criterios de aceptación
- `pj-trio --dry-run --kind cursor` (trío) y `pj-trio --dry-run --oneshot --kind cursor` imprimen el argv esperado con `--yolo`, `--trust`, `--workspace`, `--add-dir` y rutas absolutas `@<home-repo>/.cursor/rules/...`.
- `pj-trio --dry-run` (sin `--kind` o con `--kind agy`) mantiene intacto el contrato actual de `agy`.
- `trio-pm-prompt.md` incluye la mención por ruta absoluta para `wk` y `rv` cuando el trío es `cursor`, manteniendo `--timeout 600000` y verificación de `wk.md`/`rv.md`.
- `make lint` (shellcheck) y `make test` en verde en `/z/repos/ravn-trio/89-kind-cursor`.
- Subir `version` a `0.1.16` en `herdr-plugin.toml` en el mismo PR, y titular el PR con `… · v0.1.16` para que `release.yml` publique el tag/release al mergear (según `AGENTS.md` de `ravn-trio`).

## Notas de PR y Merge
- El PR se abre en `robert-flo/ravn-trio` contra `main` (`gh pr create -R robert-flo/ravn-trio`), referenciando `robert-flo/HR-CLAVIS#89` en el body (no usar `Closes #89` sin calificar porque el issue vive en `HR-CLAVIS`; usar `Closes robert-flo/HR-CLAVIS#89` o referenciarlo explícitamente).
- Redactar el body del PR con `/pr` (`## Summary`, `## Evidence`, `## Merge Danger`).
- Identidad git: ya configurada en el repo; no usar `Co-authored-by` ni trailers de agente.
