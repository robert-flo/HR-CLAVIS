# WK Handoff — robert-flo/HR-CLAVIS#92

- **Spec padre:** `robert-flo/HR-CLAVIS#85`
- **Issue:** `robert-flo/HR-CLAVIS#92`
- **PR:** https://github.com/robert-flo/pj-template/pull/90 (`robert-flo/pj-template#90`)
- **Rama:** `issue/92-adr-0023-cursor-herdr` → `main`
- **Head SHA:** `e766cb5c6e2714e433a622ef73ff6543dbef333e`
- **Worktree:** `/z/repos/pj-template/92-adr-0023-cursor-herdr`

## Archivos modificados
- `docs/adr/0025-despacho-cursor-trio-efimero-herdr.md` (nuevo)
- `docs/adr/0022-make-cursor-pm.md` (enmienda por retiro de `.cursor/agents/`)
- `docs/adr/0023-despacho-cursor-cuando-pm-en-cursor.md` (marcado superado por ADR 0025)
- `docs/adr/README.md` (índice actualizado con ADR 0025 y estados de 0022/0023)
- `AGENTS.md`
- `README.md`
- `docs/pj-template.md`

## Pruebas
- `make -C /z/repos/pj-template/92-adr-0023-cursor-herdr check` → exit 0 (`check: todo bien`: esquema `proyecto.yml` y `onboarding.yml`, idempotencia de `generar.py`, `prueba-agy.sh`, `prueba-cursor.sh`, `humo estático`, `sintaxis bash`, `YAML válido`).
- `git diff --check` → sin errores de whitespace.
- `rg -n '\.cursor/agents|Cursor IDE|subagente' AGENTS.md README.md docs/pj-template.md` → sin referencias vigentes a subagentes in-chat ni Cursor IDE como orquestador.
