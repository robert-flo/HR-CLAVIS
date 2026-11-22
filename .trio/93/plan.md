# Plan — robert-flo/HR-CLAVIS#93

- **Ticket:** `robert-flo/HR-CLAVIS#93` (parent `#85`, bloqueadores `#90` y `#92` cerrados)
- **Repo:** `robert-flo/HR-CLAVIS`
- **Worktree:** `/z/repos/HR-CLAVIS/93-sync-molde-cursor`
- **Rama:** `issue/93-sync-molde-cursor`
- **Base:** `origin/HEAD` (`master`; el PR va contra `master`)

## Objetivo

Aplicar el molde actualizado de `pj-template` en `HR-CLAVIS` vía `make sync` (actualizando `template_ref` y archivos del molde) y validar el resultado en máquina.

## Qué hacer (WK)

1. Trabajar exclusivamente en `/z/repos/HR-CLAVIS/93-sync-molde-cursor`.
2. Ejecutar `make sync` (o el flujo equivalente de actualización del molde en `HR-CLAVIS`) para actualizar `template_ref` y los archivos del molde (`pj`, `Makefile`, workflow si aplica) y regenerar `.agents/` y `.cursor/`.
3. Confirmar:
   - `.cursor/agents/` sin archivos (se eliminaron `{wk,rv,os}-hr-clavis.md` automáticamente).
   - `.cursor/rules/pm-trio-hr-clavis.mdc` generado.
   - `./pj check` pasa en verde.
4. Validar en máquina `pj-trio --dry-run --kind cursor HR-CLAVIS 93` (u otro ticket de prueba) y confirmar que genera el argv esperado (con `--yolo`, `--trust`, `--workspace`, `--add-dir`, `@<home-repo>/.cursor/rules/pm-trio-hr-clavis.mdc`, etc.).
5. Commit sin trailers `Co-authored-by` ni atribución a agentes, push de `issue/93-sync-molde-cursor` y abrir PR contra `master` con la skill `/pr` (`## Summary`, `## Evidence`, `## Merge Danger`), incluyendo:
   - Salida de `./pj check` en verde.
   - Verificación de `.cursor/agents/` vacío / inexistente y `.cursor/rules/pm-trio-hr-clavis.mdc` presente.
   - Salida de `pj-trio --dry-run --kind cursor`.
6. Escribir `/z-home-repos/HR-CLAVIS/.trio/93/wk.md` con link del PR, `head sha` y pruebas ejecutadas.

## Criterios de aceptación

- [ ] PR de `sync` / actualización de `template_ref` en `HR-CLAVIS` con `./pj check` en verde.
- [ ] Sin archivos en `.cursor/agents/` y con `.cursor/rules/pm-trio-hr-clavis.mdc` presente.
- [ ] `pj-trio --dry-run --kind cursor` verificado con salida en el PR.
