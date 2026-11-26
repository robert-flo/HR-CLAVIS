# Plan de Ejecución: robert-flo/HR-CLAVIS#239

**Spec:** robert-flo/HR-CLAVIS#239 — Centralización única de bares y worktrees (`ravn-wt` + `ravn-trio` + `pj-template`)  
**Rama integradora del spec:** `239-centralizacion-wt`  
**Referencia técnica aprobada:** `/z-home-repos/PLAN-CENTRALIZACION-WT.md`

---

## 1. Topología y Convenciones

- **Raíz única central:** `/z-home-repos` (fallback `$HOME/z-home-repos`).
- **Bares puros:** `/z-home-repos/.bare/<repo>.git`.
- **Árbol único de worktrees:** `/z-home-repos/repos/<repo>/<rama>`.
- **Invariante estricto `carpeta == rama`:**
  - Rama base: `repos/<repo>/<default-branch>` (ej. `master` o `main`)
  - Issues y tickets: `repos/<repo>/issue/<ticket>-<slug>`
  - Specs integradores: `repos/<repo>/specs/<ticket>-<slug>`
  - Revisiones RV: `repos/<repo>/rv/<pr>`
- **Home Repos:** leen directo desde `/z-home-repos/repos/<repo>/<rama-base>` sin copias ni symlinks locales.
- **Deprecación total:** `/z/bare` y `/z/repos` pasan a retiro y migración segura.

---

## 2. Orden de Ejecución de Sub-issues (DAG)

### Sub-issue 1: robert-flo/HR-CLAVIS#241
- **Título:** `ravn-git-bare-clone: centralizar set_variable.sh, soporte rutas anidadas issue/ y specs/, fetch remoto obligatorio`
- **Repo:** `robert-flo/ravn-git-bare-clone`
- **Bloqueado por:** Ninguno (puede arrancar de inmediato)
- **Criterios de Aceptación:**
  1. `runtime/helper/set_variable.sh`: SSoT fija en `/z-home-repos` (fallback `$HOME/z-home-repos`), `BARE_HOME="${RAVN_HOME_REPOS_ROOT}/.bare"` y `WORKTREES_HOME="${RAVN_HOME_REPOS_ROOT}/repos"`.
  2. `runtime/scripts/create`: fetch remoto previo obligatorio (`git fetch --prune origin` y `git remote set-head origin --auto` si base es `origin/HEAD`), soporte para carpetas anidadas (`issue/` y `specs/`) con `mkdir -p "$(dirname "$worktree_path")"`, resolución de `-r <repo>`.
  3. `runtime/scripts/issue`: convención rama y carpeta `issue/<n>-<slug>`.
  4. `runtime/scripts/clone`: puntero gitdir absoluto a `$BARE_HOME/<repo>.git`.
  5. `make test` y `make lint` pasan limpiamente sin regresiones.
  6. PR mergeado a la rama de spec en `robert-flo/ravn-git-bare-clone`.

### Sub-issue 2: robert-flo/HR-CLAVIS#242
- **Título:** `ravn-trio: adoptar raíz central única, lectura de miembros en repos/, forzar prefijo specs/ en modo spec`
- **Repo:** `robert-flo/ravn-trio`
- **Bloqueado por:** #241
- **Criterios de Aceptación:**
  1. `runtime/lib/trio_project.sh`: helper `trio_central_root()`, incluir `$(trio_central_root)/repos` en `trio_raices()`.
  2. `runtime/lib/trio_config.sh`: estandarizar `wt_prep` a `ravn-wt clone <repo>`, forzar prefijo `specs/${ticket}-<slug>` para rama y worktree cuando `modo_spec=1`, `base_ref` siempre remoto (`origin/HEAD` o `origin/specs/<rama>`).
  3. Prompts de agentes (`trio-pm-prompt.md`, `trio-os-prompt.md`): comandos usan `ravn-wt create` y convención `issue/{{TICKET}}-<slug>`.
  4. `runtime/scripts/resume`, `fleet` y `watch`: rutas resuelven contra `${WORKTREES_HOME:-$(trio_central_root)/repos}`.
  5. `make test` pasa sin errores.
  6. PR mergeado a la rama de spec en `robert-flo/ravn-trio`.

### Sub-issue 3: robert-flo/HR-CLAVIS#243
- **Título:** `pj-template: redactar ADR 0027, actualizar clone-repos.sh y reglas de agentes`
- **Repo:** `robert-flo/pj-template`
- **Bloqueado por:** #242
- **Criterios de Aceptación:**
  1. Redactar `docs/adr/0027-topologia-centralizada-ravn-wt.md` (reemplaza ADR 0019 y 0026).
  2. `clone-repos.sh`: invocar `ravn-wt clone "$repo"` contra la raíz central sin clones ni symlinks locales.
  3. Reglas de agentes (`.agents/reglas/comun.md`, `wk.md`, `rv.md`, `os.md`): eliminar bifurcación «dentro vs fuera de Home Repo», comandos de creación de worktrees a `ravn-wt create`.
  4. Pruebas: actualizar `scripts/prueba-clonar-dry-run.sh`.
  5. `make check` y tests en verde.
  6. PR mergeado a la rama de spec en `robert-flo/pj-template`.

### Sub-issue 4: robert-flo/HR-CLAVIS#244
- **Título:** `ravn-*-arch: bump y empaquetado Arch de ravn-git-bare-clone-arch y ravn-trio-arch en [omarchy-personal]`
- **Repo:** `robert-flo/ravn-git-bare-clone-arch` y `robert-flo/ravn-trio-arch`
- **Bloqueado por:** #241, #242
- **Criterios de Aceptación:**
  1. Bump de versión y tags en `ravn-git-bare-clone` y `ravn-trio`.
  2. Actualización de `PKGBUILD` y `sha256sums` en ambos repos `-arch`.
  3. Compilación con `makepkg` y publicación en el repo local `[omarchy-personal]`.
  4. Despliegue en la estación con `pacman -Syu`.
  5. `ravn-wt --help` y `pj-trio --help` reportan nueva versión y rutas centrales por defecto.

### Sub-issue 5: robert-flo/HR-CLAVIS#245
- **Título:** `SSoT y Migración: actualizar 08.3 del vault, forkhub audit y ejecutar migración en 5 fases`
- **Repo:** `robert-flo/PJ-NIRI-rf-omarchy-workstation` y Home Repos
- **Bloqueado por:** #241, #242, #243, #244
- **Criterios de Aceptación:**
  1. Actualizar `08.3 - Topologia Canonica...md` en workstation.
  2. Actualizar `forkhub audit` para inspeccionar `/z-home-repos/repos`.
  3. Ejecutar las 5 fases de migración segura de `PLAN-CENTRALIZACION-WT.md` (Fase 0 a Fase 5).
  4. Zero data loss: ningún archivo dirty ni `.env` perdido.
  5. Rutas obsoletas en cuarentena y `./pj check` pasando en todos los Home Repos.

---

## 3. Próximo Paso
Lanzar al Worker (`wk-hr-clavis-239`) para ejecutar `/implement-spec` siguiendo estrictamente el orden DAG.
