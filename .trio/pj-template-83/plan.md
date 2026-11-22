# Plan — robert-flo/pj-template#83

## Contexto
- **Issue:** `robert-flo/pj-template#83` (Parent `#81`, Blocked by `#82` ya cerrado y mergeado en `81-redisenar-makefile-hijo`).
- **Base:** `origin/81-redisenar-makefile-hijo`
- **Worktree:** `/z/repos/pj-template/83-migrar-referencias-a-c`
- **Rama:** `issue/83-migrar-referencias-a-c`
- **PR destino:** `81-redisenar-makefile-hijo` (sub-issue de `#81`)

## Qué hacer
Actualizar mensajes del lanzador `./pj`, asistente de onboarding, plantillas de reglas y fichas generadas (agy y Cursor) y aserciones de humo estático para que recomienden y validen `make a-pm` / `make c-pm` y `make listo a-pm` / `make listo c-pm` en lugar de `make agy-pm`, `make cursor-pm` o `make pm`:

1. **`plantillas/hijo/pj`**: actualizar todos los mensajes impresos por `./pj listo`, `./pj agy`, `./pj cursor` (y ayuda/comentarios si aplica) para usar `make a-pm` / `make c-pm` y `make listo a-pm` / `make listo c-pm` en lugar de `make agy-pm`, `make cursor-pm` o `make pm`.
2. **`scripts/onboard.py`** (y revisar `scripts/nuevo-hijo.sh`): actualizar los mensajes que recomiendan `make agy-pm`, `make cursor-pm` o `make pm` a la nueva convención `a-*` / `c-*` y `make listo <target>`.
3. **Reglas y plantillas de fichas (`agy` y `Cursor`)**:
   - `.agents/reglas/comun.md` y `.agents/reglas/pm.md`: reemplazar referencias a `make agy-pm`/`agy-*`, `make cursor-pm`/`cursor-*`, `make pm`/`make os` por `make a-*`, `make c-*` y `make listo a-*` / `make listo c-*`.
   - `plantillas/cursor/pm.mdc` (y revisar `plantillas/agents/*` y `plantillas/cursor/*`): alinear con los nuevos atajos `a-*` / `c-*` y `make listo <target>`.
4. **Humo estático y checks (`scripts/humo.py`, `scripts/check.sh`)**:
   - Actualizar `scripts/humo.py` (p. ej. aserciones sobre `make c-pm`, `make a-pm`, `make listo ...`, sin exigir `make cursor-pm`, `make agy-pm` ni `make pm`).
   - Revisar `scripts/check.sh` para asegurar que valide la nueva convención y no quede ninguna referencia obsoleta.

> Nota: no tocar ADRs históricos (`docs/adr/0020*`, `0022*`, `0023*`) ni `AGENTS.md`/`README.md` si no son parte de este ticket.

## Criterios de aceptación y verificación
- `./pj listo`, `./pj agy`, `./pj cursor` y `scripts/onboard.py` imprimen `make a-pm` / `make c-pm` (y `make listo a-pm` / `make listo c-pm` cuando corresponda) sin referencias a `make agy-pm`, `make cursor-pm` ni `make pm`.
- Las reglas (`.agents/reglas/*.md`) y fichas generadas (agy y Cursor) reflejan los nuevos atajos `a-*` y `c-*` y el flujo `make listo <target>`.
- `make check` (incluyendo humo estático, `scripts/prueba-agy.sh` y `scripts/prueba-cursor.sh`) pasa en verde dentro del worktree.
