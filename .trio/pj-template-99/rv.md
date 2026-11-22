# Revisión RV — robert-flo/pj-template#91 (issue robert-flo/HR-CLAVIS#99)

- **PR:** https://github.com/robert-flo/pj-template/pull/91
- **Head:** `214843a4234f8be6ee4ac2e1cc4bfff7ac3dbe9a` (`issue/99-ticket-repo-despacho` → `main`)
- **Ticket:** `robert-flo/HR-CLAVIS#99` (spec padre: `robert-flo/HR-CLAVIS#97`)
- **Veredicto:** `APRUEBO 214843a4234f8be6ee4ac2e1cc4bfff7ac3dbe9a`

## Revisión contra plan y estándares

1. **Alcance y diff:**
   - `.agents/reglas/comun.md`: agrega `[--repo <miembro> --ticket-repo <dueño/home-repo>]` en los comandos de despacho Cursor (Engineering y Trivial) e indica explícitamente pasar `--ticket-repo <dueño/home-repo>` cuando un Home Repo (`HR-*`) despacha hacia un repo miembro en agy/herdr.
   - `.agents/reglas/pm.md`: actualiza Trivial (paso 3) y Fronteras DAG (paso 6) para Cursor y agy/herdr con `[--repo <miembro> --ticket-repo <dueño/home-repo>]` y la regla explícita para `HR-*`.
   - `plantillas/cursor/pm.mdc` y `.cursor/rules/pm-pj-template.mdc`: actualizan Engineering y Trivial y añaden la viñeta `Multi-repo (HR-*)`; el archivo generado coincide byte a byte con la plantilla.
2. **Git, trailers y metadata:**
   - Base correcta (`main`), un único commit limpio (`214843a4234f8be6ee4ac2e1cc4bfff7ac3dbe9a`), autor `Roberto Flores <25asab015@ujmd.edu.sv>`, sin `Co-authored-by` ni trailers de agente, sin secretos ni imágenes.
   - Body del PR sigue `/pr` (`## Summary`, `## Evidence`, `## Merge Danger` con `Door: two-way` y `Blast Radius: Low`) y referencia `Closes robert-flo/HR-CLAVIS#99`.

## Pruebas verificadas

- **CI (`gh pr checks 91 -R robert-flo/pj-template`):** `check/check (pull_request)` en verde (43s).
- **Local en worktree `rv-91` (`214843a4234f8be6ee4ac2e1cc4bfff7ac3dbe9a`):**
  - `humo.py`: `ok   humo estático: PM/WK/RV/OS-pj-template encuentran reglas, skills y adaptadores de Cursor` (exit 0).
  - `scripts/check.sh`: todas las validaciones de esquema, idempotencia de `generar.py`, `onboard`, `clean`, `nuevo-hijo`, `dry-run`, `skills-lock`, `humo estático`, sintaxis bash y YAML en verde (al correr con `make -C` se observó un falso negativo en `make -n fleet/watch` por propagación de `MAKEFLAGS`, verificado inmediatamente en verde con `bash scripts/prueba-cursor.sh`, exit 0).
  - Worktree `rv-91` y rama local `rv/91` eliminados tras la verificación.

APRUEBO 214843a4234f8be6ee4ac2e1cc4bfff7ac3dbe9a
