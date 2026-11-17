# Handoff — HR-CLAVIS: Orquestación en `herdr` + GitHub Project #8

## Estado actual (todo cerrado y limpio en `master` / `main`)
Se completó y mergeó todo el ciclo de infraestructura base:
1. **`robert-flo/pj-template`:**
   - PR #44 (`#42`): verificación rápida `mise which agy` en `lanzar_agy()` (~0.18s vs 1m40s).
   - PR #45 (`#43`): comando manual `make sync` / `./pj sync`.
   - PR #47 (`#46`): `--dry-run`, `--no-clonar` y detección automática de worktrees en `clone-repos.sh` y `./pj listo`.
   - PR #49 (`#48`): sección `## Higiene de sesión y límites de carga` en `.agents/reglas/pm.md`.
   - PR #51 (`#50`): `clone-repos.sh` migrado a `ravn-git-bare-clone` (`.bares/` + `repos/` en Home Repos) + `/.bares/` y `/repos/` en `.gitignore`.
   - Último `main`: `da4c47e653553083d79161a92606b32809bd9d61`.
2. **`robert-flo/ravn-git-bare-clone`:**
   - PR #5 (`#4`) + release `v0.2.0`: auto-detección de Home Repo (`proyecto.yml` → `.bares/` y `repos/`) y flag `-b, --bare-dir`.
3. **`robert-flo/ravn-git-bare-clone-arch`:**
   - PR #2 (`#1`): bump a `v0.2.0` (`PKGBUILD` + `.SRCINFO`).
4. **`robert-flo/HR-CLAVIS`:**
   - PR #17 (`#16`): agregados los 8 repos nuevos a `proyecto.yml` y `AGENTS.md §3` (10 repos supervisados en total, ya clonados).
   - PR #22 (`#23` cerrado): sincronizado a `pj-template@da4c47e653553083d79161a92606b32809bd9d61`; `./pj check` (`check: todo bien`) y `./pj clonar --dry-run` pasan en verde.
   - Nota pendiente en máquina de Roberto (cuando actualice paquetes): correr `sudo pacman -Syu` / actualizar `ravn-git-bare-clone` a `0.2.0` desde `[omarchy-personal]` y opcionalmente `./pj clean` + `./pj clonar` para migrar los clones viejos de la raíz a `.bares/` + `repos/`.

## Siguiente trabajo (listo para arrancar en sesión limpia)
Ejecutar el plan aprobado en:
- `/home/niri/.gemini/antigravity-cli/brain/c1ea201c-459f-4651-99a0-4d37d054f162/plan_orquestacion_herdr.md`

Resumen de los 4 frentes del plan (hacer pruebas exhaustivas en `HR-CLAVIS` antes de propagar a `pj-template`):
1. **`robert-flo/ravn-trio` (+ `ravn-trio-arch`):** regla de `Merge Danger` (`Door: two-way` + `Blast Radius: Low` → auto-merge + auto-clean + `status.json=hecho` + cerrar workspace; otro caso → `ready-to-merge` + `needs-roberto` para merge humano) y notificaciones `herdr`.
2. **`robert-flo/ravn-sessionizer` (+ `ravn-sessionizer-arch`):** descubrir Home Repos `HR-*` (con `proyecto.yml`) junto a `pj-*` para `rs`, `ri` y `prefix+t`.
3. **Pruebas exhaustivas en `HR-CLAVIS`:** validar ticket trivial `two-way + Low`, PR `High`/`one-way` y spec multi-ticket por frontera DAG (`implement-spec` con `pj-trio --base <rama-del-spec>`) sobre `.bares/` + `repos/`.
4. **`robert-flo/pj-template`:** propagar la orquestación en `herdr` + política `Merge Danger` en `pm.md`/`comun.md`, y actualizar `.github/workflows/project-board.yml` para mapear [projects/8](https://github.com/users/robert-flo/projects/8) por Home Repo (`family.yml` + `proyecto.yml` de cada `HR-*`) en lugar de subcadenas hardcodeadas.

## Suggested skills
- `implement-spec` (para el diseño de frontera DAG con `pj-trio --base`)
- `pr` (formato `## Summary`, `## Evidence`, `## Merge Danger`)
- `code-review` (auditoría de WK antes de pasar al RV)
- `ro-restate-goals` (al retomar si Roberto ajusta el alcance)
