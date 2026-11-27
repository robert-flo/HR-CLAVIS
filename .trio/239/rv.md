# RV — spec robert-flo/HR-CLAVIS#239 (Centralización canónica de bares y worktrees)

- **Spec padre:** [robert-flo/HR-CLAVIS#239](https://github.com/robert-flo/HR-CLAVIS/issues/239)
- **PR:** [robert-flo/HR-CLAVIS#246](https://github.com/robert-flo/HR-CLAVIS/pull/246) (`239-centralizacion-wt` → `master`)
- **Head SHA:** `60a8c0cae1fb7bf9fabff0a9c3bccf628a0d9237`
- **Veredicto:** **APRUEBO 60a8c0cae1fb7bf9fabff0a9c3bccf628a0d9237**

---

## 1. Verificación de Diff y Cumplimiento del Spec

El PR implementa y documenta la culminación de la centralización canónica de repositorios bare y worktrees bajo `/z-home-repos` (ADR 0027 de pj-template), cerrando el spec #239 tras la ejecución exitosa de todos los sub-issues en orden DAG:

1. **Sub-issue 1 ([HR-CLAVIS#241](https://github.com/robert-flo/HR-CLAVIS/issues/241)): `ravn-git-bare-clone`**
   - Mergeado en [ravn-git-bare-clone#15](https://github.com/robert-flo/ravn-git-bare-clone/pull/15), tag `v0.3.1`.
   - SSoT única fijada en `/z-home-repos/.bare` y `/z-home-repos/repos` (con fallback dinámico a `$HOME/z-home-repos`).
   - Soporte automático para rutas anidadas `issue/` y `specs/` en `create` con `mkdir -p`.
   - Pre-sincronización obligatoria (`git fetch --prune origin` y `git remote set-head origin --auto`).

2. **Sub-issue 2 ([HR-CLAVIS#242](https://github.com/robert-flo/HR-CLAVIS/issues/242)): `ravn-trio`**
   - Mergeado en [ravn-trio#84](https://github.com/robert-flo/ravn-trio/pull/84), tag `v0.1.24`.
   - Helper `trio_central_root()`, resolución canónica de miembros en `repos/<repo>/<rama-base>` vía `trio_raices()`.
   - Forzado de prefijo `specs/<ticket>-<slug>` en `modo_spec=1`.
   - Prompts de arranque actualizados a comandos `ravn-wt create`.

3. **Sub-issue 3 ([HR-CLAVIS#243](https://github.com/robert-flo/HR-CLAVIS/issues/243)): `pj-template`**
   - Mergeado en [pj-template#106](https://github.com/robert-flo/pj-template/pull/106).
   - Redacción y aprobación formal del ADR 0027 (`docs/adr/0027-topologia-centralizada-ravn-wt.md`).
   - `clone-repos.sh` unificado para invocar `ravn-wt clone` contra la raíz central sin duplicación de clones locales.
   - Reglas de agentes (`comun.md`, `wk.md`, `rv.md`, `os.md`) unificadas a la nueva topología.

4. **Sub-issue 4 ([HR-CLAVIS#244](https://github.com/robert-flo/HR-CLAVIS/issues/244)): Empaquetado Arch**
   - `ravn-git-bare-clone` `0.3.1-1` y `ravn-trio` `0.1.24-1` empaquetados y desplegados en `[omarchy-personal]`.
   - Instalación activa en la estación verificada con `pacman -Syu`.

5. **Sub-issue 5 ([HR-CLAVIS#245](https://github.com/robert-flo/HR-CLAVIS/issues/245)): SSoT y Migración Segura**
   - Mergeado en [PJ-NIRI-rf-omarchy-workstation#176](https://github.com/robert-flo/PJ-NIRI-rf-omarchy-workstation/pull/176).
   - Actualización de `08.3` del vault y `forkhub audit` a profundidad 5 en `/z-home-repos/repos/`.
   - Migración en 5 fases completada: cero pérdida de datos (`.env` y cambios uncommitted preservados), 17 repositorios poblados en la nueva raíz y rutas obsoletas movidas a `/z-home-repos/.cuarentena-migracion-wt/`.

6. **Cambios en HR-CLAVIS (`AGENTS.md` y `.trio/239/`):**
   - `AGENTS.md`: Actualizado para reflejar la topología canónica bajo `/z-home-repos/.bare/` y `/z-home-repos/repos/`, versión `v0.3.1` de `ravn-git-bare-clone` (alias `ravn-wt`) y ADR 0027.
   - `.trio/239/`: Documentos de ciclo de vida (`plan.md`, `kickoff.md`, `HANDOFF.md`, `wk.md`, `status.json`) debidamente registrados.

---

## 2. Cumplimiento de Políticas y Estándares

- **Git:**
  - Sin trailers `Co-authored-by` ni firmas de agentes en los commits (`90a0274`, `60a8c0c`).
  - Autor e identidad verificados: `Roberto Flores <25asab015@ujmd.edu.sv>`.
  - Rama base: `master`.
  - Historial lineal, sin force-push, limpio y mergeable.
- **Merge Gate:**
  - `## Merge Danger`: `**Door:** two-way` · `**Blast Radius:** Low`. Califica para auto-merge por el PM.
- **Validación del repositorio:**
  - `./pj check` pasa 100% en verde (`check: todo bien`).

---

## 3. Pruebas Reales en Worktree Efímero (`rv/246`)

- Montado worktree efímero con `ravn-wt create -r /z-home-repos/repos/HR-CLAVIS -B origin/239-centralizacion-wt -b rv/246 -p '' rv/246` sobre head `60a8c0cae1fb7bf9fabff0a9c3bccf628a0d9237`.
- **`./pj check`:** ejecutado dentro del worktree efímero, completando todas las validaciones estáticas, sintácticas, esquemas y consistencia de templates (`check: todo bien`).
- **Verificación de binarios instalados:**
  - `ravn-wt --help`: confirma rutas por defecto `/z-home-repos/.bare/` y `/z-home-repos/repos/`.
  - `pj-trio kickoff HR-CLAVIS 239`: comandos renderizados validan creación de worktrees bajo `issue/` y punteros a la nueva raíz central.
- Worktree efímero y rama local/remota `rv/246` removidos tras completar la verificación.
