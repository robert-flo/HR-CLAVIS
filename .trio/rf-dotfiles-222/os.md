# OS · rf-dotfiles-222 (robert-flo/HR-CLAVIS#222)

Estado: **hecho** (auto-merge por Merge Gate).

- PR: https://github.com/robert-flo/rf-dotfiles/pull/158
- Head revisado: `c07ac343a2e8649e352df2688d143a19749b2073`
- Merge commit (squash): `16164012e2f7ada228310b6223839d1a3ca057ca`
- Merge Danger: Door: two-way · Blast Radius: Low

## Qué se hizo

`make diff` usa `chezmoi --pager "hunk pager" diff` cuando `hunk` está en el PATH, con la misma fuente canónica de antes. Si no está, deja `--no-pager`, avisa que instalarlo mejora el diff y el diff igual se imprime. `DRY_RUN=1` imprime ese comando y no corre el diff ni hunk.

## Evidencia

- Antes: la receta era siempre `chezmoi --no-pager diff`. El test nuevo fallaba 8 casos (pager, aviso y dry-run).
- Después: `bash scripts/tests/test-make-daily.sh` → `136 passed, 0 failed`. Con hunk falso, el log lleva `--pager` y `hunk pager` y el binario no se ejecuta. Sin hunk, `--no-pager`, el aviso y `DIFF`. Sin red y sin `/z/rf-dotfiles` en esa prueba.
- `make lint`: `shellcheck passed cleanly`. `make test`: `all script tests passed` (exit 0).
- En la máquina, `DRY_RUN=1 make diff` imprime `chezmoi --pager "hunk pager" diff -S /z/rf-dotfiles`. Con un PATH sin hunk, el aviso y `chezmoi --no-pager diff`.
- Repo sin CI (no hay `.github/workflows`; `statusCheckRollup` vacío). `mergeable=MERGEABLE`, `mergeStateStatus=CLEAN`.
- `code-review`: Standards, la rama duplicada de dry-run se unificó y `¿Muta?` de `make diff` sigue en No. Spec, `DRY_RUN` no corre `chezmoi diff` ni hunk; `source --canonical` sigue resolviendo el `-S` que se imprime.
- Issue 222 cerrado, sin etiquetas de triage.
