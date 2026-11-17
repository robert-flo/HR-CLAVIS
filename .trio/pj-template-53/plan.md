# Plan — robert-flo/pj-template#53

- **Ticket:** `robert-flo/pj-template#53` — `feat(os): rol OS-<proyecto> (oneshot), make agy-os y checks`
- **Parent:** Spec #52 (rama base: `52-os-y-flujo-herdr`)
- **Tipo:** Sub-issue (no pasa por RV; el WK mergea a `52-os-y-flujo-herdr` tras CI verde y `code-review` limpio)
- **Worktree:** `/z-home-repos/HR-CLAVIS/repos/pj-template/53-os-oneshot-role`
- **Rama:** `issue/53-os-oneshot-role`
- **Base / Target PR:** `52-os-y-flujo-herdr`

## Qué construir

Agregar el cuarto rol `OS-<proyecto>` (`oneshot`) al template, junto con `make agy-os` (`./pj agy os`) copiando el funcionamiento de `make agy-pm`.

1. `plantillas/roles.yml`: agregar entrada `os` con `skills: [ro-restate-goals, implement, tdd, code-review, diagnosing-bugs, pr, codebase-design]`.
2. `plantillas/agents/OS.md` y `.agents/reglas/os.md`: definir el rol `OS-<proyecto>` inspirado en `full-send`:
   - Worktree nuevo vía `ravn-git-bare-clone`, identidad git de Roberto (`Roberto Flores <25asab015@ujmd.edu.sv>`), cero trailers de agente.
   - Prueba real obligatoria (`Proof`), `code-review` limpio, PR con `/pr` (`## Summary`, `## Evidence`, `## Merge Danger`), `Babysit PR` esperando checks de CI.
   - Merge Gate: solo hace `gh pr merge <N> --squash --delete-branch --match-head-commit <sha>` si `Door: two-way` y `Blast Radius: Low` + CI verde + `code-review` limpio + `MERGEABLE` (nunca `--force` ni `--admin`). Si no cumple el gate o no puede probar, etiqueta `ready-to-merge` (o deja PR abierto) y marca `needs-roberto` en `status.json` (si corre en `herdr`).
   - Si corre en `herdr` (con `.trio/<clave>/status.json` / `trio-status`), actualiza `status.json` (`en-curso` → `hecho` | `needs-roberto`) y cierra su workspace al completar auto-merge.
3. `scripts/generar.py`, `scripts/instalar-agy.sh`, `plantillas/hijo/pj`, `plantillas/hijo/Makefile`: generar `.agents/agents/OS-<proyecto>.md`, `.cursor/agents/os-<proyecto>.md`, enlazar `OS-<proyecto>.md` en `~/.gemini/config/agents`, y agregar `make agy-os` (`./pj agy os`) copiando `make agy-pm`.
4. `scripts/check.sh` y `scripts/humo.py`: validar también `OS-<proyecto>` en el humo estático, Cursor y generación idempotente.

## Acceptance Criteria

- [ ] `make check` pasa en verde en `pj-template`.
- [ ] `HUMO_AGY=0 make humo` (y `make humo` si `agy` está disponible) pasa en verde incluyendo `OS`.
- [ ] PR abierto contra `52-os-y-flujo-herdr` y mergeado por el WK tras CI verde y `code-review` limpio.
