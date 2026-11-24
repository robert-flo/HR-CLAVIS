# WK — Spec robert-flo/HR-CLAVIS#170

- **PR final:** https://github.com/robert-flo/pj-template/pull/99 (`170-makefile-modular-rollo` → `main`)
- **Head SHA:** `306e9fe72e8377a40b51f625f17c80419f52a2e1`
- **Sub-PRs mergeados:**
  - https://github.com/robert-flo/pj-template/pull/98 (cierra `robert-flo/HR-CLAVIS#177`)

## Archivos tocados

- `Makefile` (`run_op` corregido con `printf` en comillas simples + escape de `'` para `DRY_RUN=1`)
- `make/help.mk`
- `make/template.mk`
- `make/hijo.mk`
- `template-paths.txt`

## Pruebas verificadas

- `make -n DRY_RUN=1 PJ_RAIZ=/tmp/no-existe clean` → `printf '\033[33m[DRY_RUN]\033[0m %s\n' 'cd "/tmp/no-existe" && ./pj clean '` (exit `0`).
- `make DRY_RUN=1 PJ_RAIZ=/tmp/no-existe clean` → imprime `[DRY_RUN] cd "/tmp/no-existe" && ./pj clean ` sin ejecutar `cd` ni `./pj clean` (exit `0`).
- `make DRY_RUN=1 skills humo clonar nuevo-hijo` → imprime `[DRY_RUN]` literal sin romper comillas ni expandir `$${...}` (exit `0`).
- `MAKEFLAGS= HUMO_AGY=0 make check` en `pj-template`: `check: todo bien` (exit `0`).
- `PJ_TEMPLATE_DIR=/z-home-repos/HR-CLAVIS/repos/pj-template/170-makefile-modular-rollo ./pj check` desde `/z-home-repos/HR-CLAVIS`: `check: todo bien` (exit `0`).
- `git diff origin/main -- plantillas/hijo/Makefile`: 0 líneas (intacto).
