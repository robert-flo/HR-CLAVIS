# Plan — Ticket robert-flo/HR-CLAVIS#138

- **Issue:** `robert-flo/HR-CLAVIS#138` — `ravn-trio: barrera interactive_ready y verificación/reintento en enviar_kickoff`
- **Repo de código:** `robert-flo/ravn-trio`
- **Worktree:** `/z/repos/ravn-trio/138-interactive-ready-kickoff`
- **Rama:** `issue/138-interactive-ready-kickoff`
- **Base:** `origin/HEAD` (`main`)
- **PR destino:** `main` en `robert-flo/ravn-trio`
- **Versión (`herdr-plugin.toml`):** `0.1.18` → `0.1.19` (título del PR debe terminar en `· v0.1.19` según `ravn-trio/AGENTS.md`)

## Qué construir

Blindar el arranque de agentes `agy` en `ravn-trio` (`trio_agy.sh` y `trio_herdr.sh`) para evitar el race donde `herdr agent prompt` envía el kickoff antes de que `agy` termine de abrir la caja de entrada (dejando texto atascado sin Enter):

1. En `esperar_agente_listo` (`trio_agy.sh`): para `agy`, exigir `agent_status == "idle"`, `interactive_ready == true` en `herdr agent get` y que `herdr agent read` muestre el prompt `>` listo (sin diálogo de confianza).
2. En `enviar_kickoff` (`trio_herdr.sh`): tras enviar `herdr agent prompt`, verificar (~1–2s después) con `herdr agent get` / `herdr agent read` que el agente salió de `idle` o muestra actividad (`Generating...`); si sigue `idle` con texto atascado en la caja `>`, limpiar la línea (`Ctrl+U`) y reintentar 1–2 veces. Si tras los reintentos sigue sin arrancar, actualizar `status.json` a `blocked` con el error para que `pj-trio watch` despierte de inmediato.

## Criterios de aceptación

- [ ] `esperar_agente_listo` no retorna `0` hasta que `agent_status == "idle"`, `interactive_ready == true` y el prompt `>` está presente, verificado con tests CLI usando stub de `herdr`.
- [ ] `enviar_kickoff` detecta prompt no procesado, limpia/reintenta y marca `status.json` como `blocked` si agota reintentos, verificado con tests CLI.
- [ ] `make lint` y `make test` pasan en verde en `ravn-trio` y su salida se incluye en el PR.

## Reglas para el WK y PR

- Trabajar **solo** en `/z/repos/ravn-trio/138-interactive-ready-kickoff`.
- Leer `ravn-trio/AGENTS.md` antes de editar.
- No incluir trailers `Co-authored-by` ni atribución a agentes en commits.
- Abrir el PR en `robert-flo/ravn-trio` contra `main` usando la skill `/pr` (`## Summary`, `## Evidence`, `## Merge Danger`).
- Referenciar `robert-flo/HR-CLAVIS#138` en el body del PR (no `Closes #138`, ya que el issue vive en `HR-CLAVIS`).
- Subir `version` en `herdr-plugin.toml` a `0.1.19` y usar `· v0.1.19` en el título del PR.
