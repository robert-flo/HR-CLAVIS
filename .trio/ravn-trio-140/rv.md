# RV — robert-flo/HR-CLAVIS#140

- **PR:** https://github.com/robert-flo/ravn-trio/pull/70
- **Head:** `dbb4622f896a8ee26093904d7d19ecb19ef15f89`
- **Veredicto:** APRUEBO `dbb4622f896a8ee26093904d7d19ecb19ef15f89`

## Qué se revisó

- `trio_nombres` resuelve `BARE_HOME` y `WORKTREES_HOME` al Home Repo cuando existe `.bares/` o `repos/`, y deja `/z/bare` + `/z/repos` fuera de un Home Repo. Un override explícito gana.
- El kickoff unitario, el oneshot y el modo spec usan `WT_PREP`: `fetch` si el bare ya existe, `ravn-git-bare-clone clone` con esas variables si falta.
- `herdr-plugin.toml` queda en `0.1.20`.
- CI verde en ese head: ShellCheck Lint, Test Suite, Test Installation Target.

El agente RV de agy no pudo revisar: cuota individual agotada. La revisión de este head se hizo en esta sesión, a pedido de Roberto de terminar el ticket.
