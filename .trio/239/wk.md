# Reporte Final de Implementación: robert-flo/HR-CLAVIS#239

**Spec:** robert-flo/HR-CLAVIS#239 — Centralización única de bares y worktrees (`ravn-wt` + `ravn-trio` + `pj-template`)  
**Rama integradora del spec:** `239-centralizacion-wt`  
**PR Final:** https://github.com/robert-flo/HR-CLAVIS/pull/246  
**Fecha:** 2026-11-26  
**Worker:** WK-HR-CLAVIS (`wk-hr-clavis-239`)  

---

## 1. Resumen de Implementación por Sub-issue (DAG)

### Sub-issue 1: robert-flo/HR-CLAVIS#241
- **Repo:** `robert-flo/ravn-git-bare-clone`
- **PR:** https://github.com/robert-flo/ravn-git-bare-clone/pull/15 (mergeado a `main`)
- **Release/Tag:** `v0.3.1` (commit `ab1649b`)
- **Archivos tocados:** `runtime/helper/set_variable.sh`, `runtime/scripts/create`, `runtime/scripts/issue`, `runtime/scripts/clone`, `tests/*`.
- **Resumen:** SSoT única fijada en `/z-home-repos/.bare` y `/z-home-repos/repos` (con fallback dinámico a `$HOME/z-home-repos`). Soporte de subdirectorios anidados `issue/` y `specs/` en `create` con `mkdir -p` automático. Pre-sincronización remota obligatoria antes de crear worktrees (`git fetch --prune origin` y `git remote set-head origin --auto`). Convención estricta `carpeta == rama` (`issue/<n>-<slug>`).

### Sub-issue 2: robert-flo/HR-CLAVIS#242
- **Repo:** `robert-flo/ravn-trio`
- **PR:** https://github.com/robert-flo/ravn-trio/pull/84 (mergeado a `main`)
- **Release/Tag:** `v0.1.24` (commit `22cf838`)
- **Archivos tocados:** `runtime/lib/trio_project.sh`, `runtime/lib/trio_config.sh`, `trio-pm-prompt.md`, `trio-os-prompt.md`, `runtime/scripts/{fleet,resume}`.
- **Resumen:** Adopción de helper `trio_central_root()`, resolución de repos miembros en `repos/<repo>/<rama-base>` vía `trio_raices()`, forzado de prefijo `specs/<ticket>-<slug>` en `modo_spec=1`, estandarización de `wt_prep` a `ravn-wt clone`, prompts actualizados a comandos `ravn-wt create`.

### Sub-issue 3: robert-flo/HR-CLAVIS#243
- **Repo:** `robert-flo/pj-template`
- **PR:** https://github.com/robert-flo/pj-template/pull/106 (mergeado a `main`)
- **Archivos tocados:** `docs/adr/0027-topologia-centralizada-ravn-wt.md`, `clone-repos.sh`, `.agents/reglas/{comun,wk,rv,os}.md`, `scripts/prueba-clonar-dry-run.sh`.
- **Resumen:** Redacción y aprobación formal del ADR 0027 (reemplaza ADR 0019 y 0026). `clone-repos.sh` actualizado para invocar limpiamente `ravn-wt clone` contra la raíz central sin copias ni symlinks locales. Unificación de reglas de agentes eliminando bifurcaciones de Home Repos.

### Sub-issue 4: robert-flo/HR-CLAVIS#244
- **Repos:** `robert-flo/ravn-git-bare-clone-arch`, `robert-flo/ravn-trio-arch`, `robert-flo/omarchy-personal-repo`
- **Commits:** `ravn-git-bare-clone-arch` (`9935830`), `ravn-trio-arch` (`6fa2c78`), `omarchy-personal-repo` (`45893195916`)
- **Paquetes instalados:** `ravn-git-bare-clone 0.3.1-1` y `ravn-trio 0.1.24-1`
- **Resumen:** Bump de versión y tags upstream (`v0.3.1` y `v0.1.24`). Actualización de `PKGBUILD` y `sha256sums` en ambos repos `-arch`. Compilación y publicación desatendida en `[omarchy-personal]`. Despliegue en la estación con `pacman -Syu`. Verificación de comandos `--help` reportando rutas canónicas por defecto.

### Sub-issue 5: robert-flo/HR-CLAVIS#245
- **Repo:** `robert-flo/PJ-NIRI-rf-omarchy-workstation` y Home Repos
- **PR:** https://github.com/robert-flo/PJ-NIRI-rf-omarchy-workstation/pull/176 (mergeado a `master`)
- **Resumen:** Actualización de SSoT en el vault (`08.3 - Topologia Canonica...md`) y `forkhub audit` a profundidad 5 para `/z-home-repos/repos/`. Ejecución completa de las 5 fases de migración segura:
  - Fase 0: Auditoría exhaustiva de uncommitted/unpushed.
  - Fase 2: Poblado limpio de 17 repositorios en `/z-home-repos/.bare/` y `/z-home-repos/repos/` con `ravn-wt clone`.
  - Fase 3: Rescate de archivos `.env` (monorepo Cuéntame) y uncommitted de workstation (Zero Data Loss).
  - Fase 4: Validación integral (`pj-trio kickoff`, `./pj check` en verde en los 3 Home Repos).
  - Fase 5: Traslado de rutas obsoletas (`/z/bare`, `/z/repos`, `<HR>/.bares`, `<HR>/repos`, clones redundantes) a cuarentena en `/z-home-repos/.cuarentena-migracion-wt/`.

---

## 2. Evidencias y Pruebas Reales

1. **`ravn-wt --help`:**
   ```text
   Bare objects:  $BARE_HOME/<repo>.git   (default: /z-home-repos/.bare/<repo>.git)
   Worktrees:     $WORKTREES_HOME/<repo>  (default: /z-home-repos/repos/<repo>)
   ```
2. **`pj-trio --help` y `kickoff`:**
   Rutas resuelven contra `/z-home-repos/.bare/` y `/z-home-repos/repos/`, con comandos `ravn-wt create -r /z-home-repos/repos/<repo> -B origin/HEAD -b issue/<n>-<slug> -p '' issue/<n>-<slug>`.
3. **`forkhub audit`:**
   168 repositorios auditados bajo la nueva topología canónica sin discrepancias ni repos huérfanos.
4. **`./pj check`:**
   - `HR-CLAVIS`: `check: todo bien` (100% pasando).
   - `HR-GIT-SETUP`: `check: todo bien` (100% pasando).
   - `HR-KODIGO-GRUPO-12`: `check: todo bien` (100% pasando).
5. **Workstation PR #176 CI:**
   - `CI/Validación del Vault`: verde (0 errores críticos, 67 notas analizadas).
   - `CI/ShellCheck`: verde (0 advertencias).
   - `CI/Idempotencia de Onboarding`: verde.
   - `CI/Tests de secretos`: verde.

---

## 3. Estado de la Rama y Entrega

- Rama del spec: `239-centralizacion-wt`
- Head SHA: `6c526f48adecb17871586b3949a1c2f6e5b94b52`
- PR Final: https://github.com/robert-flo/HR-CLAVIS/pull/246
- Estado del árbol de trabajo: limpio, merge con `origin/master` resuelto sin conflictos, listo para revisión del RV.
