# Reporte OS: ravn-git-bare-clone#9

- **Ticket:** [robert-flo/ravn-git-bare-clone#9](https://github.com/robert-flo/ravn-git-bare-clone/issues/9)
- **PR:** [#10](https://github.com/robert-flo/ravn-git-bare-clone/pull/10)
- **Head commit:** `d69b9c47858eb2627088a286a5bc83ec5a862832`
- **Merge SHA:** `87e8d54936fbcef4e66c729c2316aa7d0941566d`
- **Merge Danger:** `Door: two-way` / `Blast Radius: Low`
- **Estado:** Merged y cerrado con éxito vía Merge Gate (auto-merge).

## Resumen del cambio
Se corrigieron las rutas de verificación en el job `install` de `.github/workflows/ci.yml`. Tras el renombre del launcher y el cambio de default `PREFIX` a `/opt/ravn-wt` (PR #6), las pruebas de instalación requerían validar la existencia de `dest/opt/ravn-wt/ravn-wt`, su biblioteca `global_fn.sh`, los symlinks en `dest/usr/bin/` (`ravn-wt`, `ravn-git-bare-clone`, `git-bare-clone`, `git-create-worktree`, `git-issue-worktree`) y las completions en `dest/opt/ravn-wt/runtime/completion/`.

## Evidencia de pruebas
- **Instalación local y verificación de archivos:**
  ```text
  $ mkdir -p dest && make DESTDIR=dest install
  $ test -x dest/opt/ravn-wt/ravn-wt
  $ test -f dest/opt/ravn-wt/runtime/lib/global_fn.sh
  $ test -L dest/usr/bin/ravn-wt
  $ test -L dest/usr/bin/ravn-git-bare-clone
  $ test -L dest/usr/bin/git-bare-clone
  $ test -L dest/usr/bin/git-create-worktree
  $ test -L dest/usr/bin/git-issue-worktree
  $ test -f dest/opt/ravn-wt/runtime/completion/git-bare-clone.bash
  $ test -f dest/opt/ravn-wt/runtime/completion/_git-bare-clone
  Resultado: OK (exited 0)
  ```
- **Pruebas y linters (`make check`):**
  - ShellCheck: limpio en todos los scripts.
  - Test suite (`bash tests/run.sh`): todos los tests pasando exitosamente.
- **CI en GitHub:**
  - `ShellCheck Lint`: SUCCESS
  - `Test Suite`: SUCCESS
  - `Test Installation Target`: SUCCESS
