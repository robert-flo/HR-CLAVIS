# Plan: robert-flo/pj-template#82 — Rediseñar Makefile del hijo (a-*, c-*, help agrupado) y su suite de pruebas

- **Ticket:** https://github.com/robert-flo/pj-template/issues/82 (sub-issue de #81)
- **Repo:** `robert-flo/pj-template`
- **Worktree:** `/z-home-repos/HR-CLAVIS/repos/pj-template/82-redisenar-makefile-hijo`
- **Rama del WK:** `issue/82-redisenar-makefile-hijo`
- **Base / PR target:** `81-redisenar-makefile-hijo`

## Qué hay que hacer
1. Rediseñar `plantillas/hijo/Makefile`:
   - Arranque rápido por defecto (`--solo`) en todos los targets de agentes.
   - Convención simétrica para ambas plataformas:
     - `a-pm`, `a-wk`, `a-rv`, `a-os` → `./pj agy <rol> --solo $(ARGS)`
     - `c-pm`, `c-wk`, `c-rv`, `c-os` → `./pj cursor <rol> --solo $(ARGS)`
   - Eliminar los aliases sin prefijo (`pm`, `os`) y los targets duplicados con `pull + listo` (`agy-*`, `cursor-*`).
   - Mantener intactos los targets de mantenimiento y flota: `listo`, `onboard`, `clean`, `sync`, `check`, `fleet`, `watch`.
   - Implementar `make` (`help`, `.DEFAULT_GOAL := help`) agrupado en 4 secciones exactas:
     - `Agentes — agy`
     - `Agentes — Cursor`
     - `Home Repo`
     - `Flota (pj-trio)`
     con los 15 targets operativos y descripciones autocontenidas sin referencias cruzadas (e.g. nada de "Igual, con el WK").
2. Actualizar la suite de pruebas en `scripts/` (`prueba-agy.sh`, `prueba-cursor.sh`, `check.sh`, y cualquier otra referencia relevante en pruebas) para verificar el nuevo contrato externo de punta a punta:
   - `make` (`help`) imprime las 4 secciones, los 15 targets operativos y descripciones autocontenidas.
   - `make -n` y ejecución con stubs confirman que `a-*` y `c-*` invocan `./pj agy <rol> --solo` y `./pj cursor <rol> --solo` pasando `ARGS`, sin ejecutar `git pull` ni `./pj listo`.
   - `make listo a-pm` y `make listo c-pm` actualizan el hijo y abren el agente.
   - `fleet` y `watch` conservan su invocación a `pj-trio`.
   - `make check` en `pj-template` pasa completo en verde.
3. Si en `pj-template` o en `plantillas/hijo/` hay validación del molde en `check.sh` (por ejemplo comparando `Makefile` del hijo de prueba con `plantillas/hijo/Makefile`), asegurarse de que todo pasa limpio.
   - Nota de alcance: la actualización de mensajes impresos/generados (`./pj`, `onboarding.py`, `comun.md`, docs/ADRs) corresponde a otros sub-issues de #81 si aplican, salvo lo estrictamente necesario para que `make check` de `pj-template` pase en verde sobre esta rama.

## Criterios de aceptación
- `make` (`help`) imprime las 4 secciones (`Agentes — agy`, `Agentes — Cursor`, `Home Repo`, `Flota (pj-trio)`), los 15 targets operativos (`a-{pm,wk,rv,os}`, `c-{pm,wk,rv,os}`, `listo`, `onboard`, `sync`, `check`, `clean`, `fleet`, `watch`) y descripciones autocontenidas sin referencias cruzadas.
- `make -n` y ejecución con stubs confirman `a-*` → `./pj agy <rol> --solo` y `c-*` → `./pj cursor <rol> --solo` con `ARGS`, sin `git pull` ni `./pj listo`.
- `make listo a-pm` y `make listo c-pm` actualizan y abren el agente; `fleet` y `watch` conservan `pj-trio`.
- `make check` en `pj-template` (incluyendo pruebas de `agy` y `Cursor`) pasa en verde.

## Flujo de entrega (sub-issue)
- Trabajar SOLO en `/z-home-repos/HR-CLAVIS/repos/pj-template/82-redisenar-makefile-hijo`.
- Abrir PR contra `81-redisenar-makefile-hijo` con `Closes #82` en el body (redactado con skill `pr`).
- Con CI verde y `code-review` limpio, mergear el PR a `81-redisenar-makefile-hijo` con `--squash --delete-branch --match-head-commit <sha-verde>` y anotar todo en `wk.md`.
