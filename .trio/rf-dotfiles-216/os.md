# OS · rf-dotfiles-216 (robert-flo/HR-CLAVIS#216)

Estado: **hecho** (auto-merge por Merge Gate).

- PR: https://github.com/robert-flo/rf-dotfiles/pull/155
- Head revisado: `29776159687f0cabd3547383b8e45b4e802cba38`
- Merge commit (squash): `1f1494df214df9a47e07bc742f79d908c1eaca64`
- Merge Danger: Door: two-way · Blast Radius: Low

## Qué se hizo

`make status` y `make morning` piden `rf-make-daily.sh source --canonical`: `readlink -f` de `chezmoi source-path` cuando ese directorio existe. Un `run_onchange` que incluye `.chezmoi.sourceDir` cambia de hash si `-S` es el enlace y el apply se hizo por la ruta real, y chezmoi lo lista como `R`. `make apply` no cambió. `safe`, `root`, `diff` y `lose` siguen con el enlace.

## Evidencia

- Antes, usuario `niri`: `chezmoi status -S "$(chezmoi source-path)"` listaba `R 02-install-system-tools.sh` y `R install-brief-table.sh`. Con `readlink -f` no.
- Después, `make status` en el worktree: esas `R` ya no salen (`R_COUNT=0`) y siguen los `MM` reales (`MM_COUNT=4`: `config.json`, `ui-preferences.json`, `.config/cursor`, `statindet-colors.kdl`).
- `bash scripts/tests/test-make-daily.sh`: 107 passed, 0 failed. El caso nuevo aplica con chezmoi real sobre un directorio y un symlink.
- `make lint` limpio. `make test` en verde.
- Repo sin CI (no hay `.github/workflows`; `gh pr checks` dice «no checks reported»). `mergeable=MERGEABLE`, `mergeStateStatus=CLEAN`.
- `code-review`: Standards sin violaciones; Spec limpio después de limitar `--canonical` a status y morning.
- Issue 216 cerrado. Etiqueta `ready-for-agent` quitada.
