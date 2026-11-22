# Plan — robert-flo/HR-CLAVIS#92

- **Issue:** `robert-flo/HR-CLAVIS#92` (Spec padre: `robert-flo/HR-CLAVIS#85`)
- **Repo / PR:** `robert-flo/pj-template`
- **Worktree:** `/z/repos/pj-template/92-adr-0023-cursor-herdr`
- **Rama:** `issue/92-adr-0023-cursor-herdr`
- **Base:** `origin/HEAD` (`main`)

## Qué construir
Documentar el reemplazo del despacho in-chat en Cursor por tríos efímeros en `herdr`:

1. Publicar el nuevo **ADR 0025** en `docs/adr/` que supera el **ADR 0023** (`0023-despacho-cursor-cuando-pm-en-cursor.md`) y enmienda el **ADR 0022** (`0022-make-cursor-pm.md`) — conservando `make c-pm`, `cursor-agent --yolo --trust`, `mise` y `@.cursor/rules/pm-<slug>.mdc`, y retirando los subagentes in-chat `.cursor/agents/*`.
2. Actualizar `docs/adr/README.md` (índice de ADRs) y marcar ADR 0022 y ADR 0023 según su estado vigente (enmendado / superado por ADR 0025).
3. Actualizar `AGENTS.md`, `README.md` y `docs/pj-template.md` para reflejar:
   - `pj-trio --kind cursor`
   - `pm-trio-<slug>.mdc`
   - eliminación de `.cursor/agents/*` (subagentes in-chat)
   - Cursor IDE queda fuera de alcance como orquestador de tríos.

## Criterios de aceptación
- [ ] Nuevo ADR (0025) publicado y enlazado en `docs/adr/README.md`; ADR 0022 y ADR 0023 marcados según su estado vigente.
- [ ] `AGENTS.md`, `README.md` y `docs/pj-template.md` sin referencias vigentes a subagentes in-chat `.cursor/agents/*`.
- [ ] `make check` en `pj-template` pasa en verde.
- [ ] PR abierto contra `main` con `## Summary`, `## Evidence` y `## Merge Danger` (skill `/pr`), sin trailers ni atribución a agentes en los commits.
