# OS — robert-flo/HR-CLAVIS#141

- **PR:** https://github.com/robert-flo/pj-template/pull/96
- **Head:** `2b0b13578eb1cf29c4881bc0911c216372c53d03`
- **Merge:** `cb71f1987756faeb768dfdb6ae9f1fb568cee33e`
- **Worktree:** `/z/repos/pj-template/141-home-repo-reglas`

## Qué quedó

`wk.md`, `rv.md` y `comun.md` mandan, dentro de un Home Repo, `BARE_HOME=<home>/.bares` y `WORKTREES_HOME=<home>/repos`. Si el bare ya existe, `ravn-git-bare-clone clone` solo hace fetch y no duplica en `/z`. ADR 0026 enmienda 0019 para ese caso. No existe `plantillas/reglas/`; la fuente versionada es `.agents/reglas/`.

## Pruebas

`make check` en el worktree terminó en verde (`check: todo bien`) antes y después del ADR. CI del PR: check SUCCESS.

## code-review

El primer pase marcó el choque con ADR 0019 y que un `fetch` suelto no crea la raíz que `git-create-worktree -r` exige. Se corrigió con ADR 0026 y el clone con `BARE_HOME` / `WORKTREES_HOME`. El pedido de pegar `make check` en el PR no era un hueco del diff: va en el body.

## Merge Danger

two-way / Low. Auto-merge sobre el head aprobado por el gate.
