# Plan del Spec robert-flo/HR-CLAVIS#170

- **Repo de trabajo:** `robert-flo/pj-template` (rama base: `main`)
- **Rama del spec:** `170-makefile-modular-rollo`
- **Issue spec:** https://github.com/robert-flo/HR-CLAVIS/issues/170

## Orden de sub-issues (DAG)

1. **`robert-flo/HR-CLAVIS#177`** — `pj-template: modularizar Makefile raíz con $(TPL)/make/*.mk manteniendo plantillas/hijo/Makefile intacto`
   - **Bloqueado por:** ninguno (inicia inmediato)
   - **Qué construir:**
     - Actualizar el `Makefile` raíz de `pj-template` a una estructura modular estilo Rollo (`make/help.mk`, `make/template.mk`, `make/hijo.mk`) incluidos vía `include $(TPL)/make/*.mk` para que las invocaciones desde Home Repos hijos (`./pj <target>`) resuelvan limpio.
     - Mantener `TPL`, `PJ_RAIZ`, `PY`, los 10 targets existentes y `plantillas/hijo/Makefile` completamente intactos.
     - Soportar `DRY_RUN=1` en targets operacionales (`skills`, `humo`, `clonar`, `clean`, `nuevo-hijo`).
   - **Criterios de aceptación:**
     - `plantillas/hijo/Makefile` tiene cero diff contra `main`.
     - El `Makefile` raíz usa `include $(TPL)/make/*.mk` y preserva `TPL`, `PJ_RAIZ`, y `PY`.
     - `make help`, `make check`, y `make generar` pasan en la raíz de `pj-template`; `DRY_RUN=1` funciona en targets operacionales.
     - Correr `./pj check` desde un Home Repo hijo con `PJ_TEMPLATE_DIR` apuntando al worktree pasa en verde.

## Pruebas requeridas para el PR final (`170-makefile-modular-rollo` -> `main`)

- `make help`, `make check`, `make generar` en verde en la raíz de `pj-template`.
- `./pj check` desde un Home Repo hijo con `PJ_TEMPLATE_DIR` apuntando al worktree en verde.
- `git diff origin/main -- plantillas/hijo/Makefile` vacío.
