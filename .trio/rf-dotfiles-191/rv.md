# RV — robert-flo/rf-dotfiles#140

- **Spec:** https://github.com/robert-flo/HR-CLAVIS/issues/191
- **PR:** https://github.com/robert-flo/rf-dotfiles/pull/140 (`191-makefile-util-en-el-dia` → `191-chezmoi-make-diario`)
- **Head:** `46358c5fdf828e823b8f2678f4c05edb0b51a0db`
- **Plan:** `.trio/rf-dotfiles-191/plan.md` no está en el handoff. La revisión usa el cuerpo del issue #191.

## Veredicto

**APRUEBO** `46358c5fdf828e823b8f2678f4c05edb0b51a0db`

## Qué cubre el diff

Los cinco targets y el cambio de `status`/`diff` están en `make/daily.mk` y `scripts/rf-make-daily.sh`, contra `chezmoi source-path` y el mismo perfil que `.chezmoiignore.tmpl` (`CHEZMOI_PROFILE`, si no `niri`/`niri-classic`/`niri-test`/`sura` → `clavis`, nyxuri → `nyxuri`, `niri-upstream` → `clavis-upstream`, el resto → `omarchy`).

- `morning` hace fetch si puede, imprime el tablero igual si el fetch falla, y distingue `same` / `behind` / `ahead`.
- `lose` lista solo la segunda columna `M` o `D` y sale distinto de cero si hay alguno. `R` queda en `scripts`.
- `pull` es `git pull --rebase --autostash origin master`, una vez si la copia y el clon son el mismo directorio, y no llama `apply`.
- `safe` aplica con el perfil resuelto y `-S` de esa copia solo cuando `lose` sale vacío.
- `root` con `sudo -n` renderiza los dos scripts de raíz y no llama `apply`. Sin `sudo -n` sale distinto de cero y no dice que instaló.
- `status` y `diff` usan `-S` de la copia de producción. `apply` sigue en el clon invocador, como pide el spec.
- `DRY_RUN=1` no hace pull, apply ni execute de los scripts.
- El diff no toca plantillas, el perfil `clavis` ni los `yt-*`. Sin `Co-authored-by`. Door two-way, Blast Radius Low.

## Prueba en `rv/140`

Worktree `repos/rf-dotfiles/rv-140` en el head exacto.

- `make lint`: shellcheck limpio.
- `make test`: `scripts/tests/test-make-daily.sh` 63 passed, 0 failed; `test-yt-video-playlist.sh` 32 PASS; `test-yt-video.sh` 22 PASS.
- `chezmoi status --color=false` en esta máquina usa dos columnas (` M`, `MM`, ` R`), el formato que clasifica `rf_classify`.
- Copia local un commit adelante de `origin/master`: `behind: 0`, `ahead: 1`, `same: no`.
- `DRY_RUN=1 make morning` imprime el fetch y no lo corre.
- `chezmoi execute-template --file … -S <worktree>` de los dos `.tmpl` de raíz sale con shebang bash y cero `{{`. No se ejecutó.

## Observación que no bloquea

Con `DRY_RUN=1`, `morning` deja `fetch: no` aunque el fetch no falló: se omitió. El comando sí se imprime y no hay fetch.
