# WK — Spec robert-flo/HR-CLAVIS#191

- **Spec:** `robert-flo/HR-CLAVIS#191`
- **PR final:** https://github.com/robert-flo/rf-dotfiles/pull/140 (`191-makefile-util-en-el-dia` → `191-chezmoi-make-diario`)
- **Head SHA:** `46358c5fdf828e823b8f2678f4c05edb0b51a0db`

## Sub-issues mergeados a `191-makefile-util-en-el-dia`

- `#192` → https://github.com/robert-flo/rf-dotfiles/pull/137 (`d9e641fa0f418ec5cb39387f52c49f19b1270420`)
- `#193` → https://github.com/robert-flo/rf-dotfiles/pull/138 (`64e54a766e387baab557c2cc08d78adce3ed3e65`)
- Arreglo del code-review de la rama → https://github.com/robert-flo/rf-dotfiles/pull/139 (`46358c5fdf828e823b8f2678f4c05edb0b51a0db`)

## Pruebas

- `make test` en el worktree del arreglo: 60 PASS en `scripts/tests/test-make-daily.sh` (git temporal, chezmoi falso, dos directorios) y los tests de `yt-*` en verde (`test-yt-video-playlist.sh` 32 PASS, `test-yt-video.sh` 22 PASS).
- `make lint`: shellcheck limpio.
- `chezmoi execute-template --file` de `run_onchange_before_01-install-packages.sh.tmpl` y `run_onchange_after_02-install-system-tools.sh.tmpl` renderiza bash. No se ejecutó.

Worktrees de `#192`, `#193` y del arreglo eliminados.
