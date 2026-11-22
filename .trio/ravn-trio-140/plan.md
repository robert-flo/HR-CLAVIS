# Plan — robert-flo/HR-CLAVIS#140

- **Ticket:** `robert-flo/HR-CLAVIS#140` — *ravn-trio: usar .bares/ y repos/ locales del Home Repo sin reclonar*
- **Repo de trabajo (PR):** `robert-flo/ravn-trio` (rama base `main`)
- **Worktree:** `/z/repos/ravn-trio/140-home-repo-bares-repos`
- **Rama:** `issue/140-home-repo-bares-repos` (base `origin/HEAD`, commit `984e699`)

## Qué construir

1. **`runtime/lib/trio_project.sh` y `runtime/lib/trio_config.sh`:**
   - Cuando `$project_dir` tiene `.bares/` o `repos/` (Home Repo), resolver:
     - `BARE_HOME="${project_dir}/.bares"`
     - `WORKTREES_HOME="${project_dir}/repos"`
     - `wt_raiz="${project_dir}/repos/${nombre_wt}"`
   - Mantener `/z/bare` + `/z/repos` como fallback solo fuera de un Home Repo (o respetar overrides explícitos si aplica según la lógica existente).
2. **`trio-os-prompt.md`, `trio-pm-prompt.md` y `runtime/lib/trio_config.sh`:**
   - Pasar/usar `BARE_HOME` y `WORKTREES_HOME` del Home Repo (placeholders si hace falta).
   - Si `${project_dir}/.bares/${nombre_wt}.git` (el bare del repo miembro) ya existe, no reclonar: ejecutar `ravn-git-bare-clone clone` solo si falta el bare; de lo contrario, hacer solo `git -C <bare_dir> fetch origin` + `git-create-worktree`.
3. **Versión (`ravn-trio/AGENTS.md`):**
   - Subir `version` en `herdr-plugin.toml` de `0.1.19` a `0.1.20` en el mismo PR, y usar título `… · v0.1.20`.

## Criterios de aceptación

- [ ] En un Home Repo con `.bares/` y `repos/`, `pj-trio` resuelve `WT_RAIZ` a `<project_dir>/repos/<nombre_wt>` y `BARE_HOME` a `<project_dir>/.bares`, verificado con tests CLI.
- [ ] Si `<project_dir>/.bares/<repo>.git` ya existe, los prompts no reclonan en `/z/bare/`, verificado con test CLI / `--print-kickoff`.
- [ ] `make lint` y `make test` pasan en verde en `ravn-trio` y su salida se incluye en el PR.

## Reglas para el WK

- Trabajar **exclusivamente** en `/z/repos/ravn-trio/140-home-repo-bares-repos`.
- Redactar el PR con la skill `/pr` (`## Summary`, `## Evidence`, `## Merge Danger`), contra `main`, referenciando `robert-flo/HR-CLAVIS#140` en el body (no `Closes #140`).
- Escribir `/z-home-repos/HR-CLAVIS/.trio/ravn-trio-140/wk.md` con URL del PR, `head` SHA y resultado de las pruebas (`make lint` y `make test`).
