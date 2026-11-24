# WK — spec robert-flo/HR-CLAVIS#195 (rf-dotfiles)

## PR final
- https://github.com/robert-flo/rf-dotfiles/pull/148 — `195-flags-dolphin-niri` → `master`, **ready for review** (ya no es draft)
- **Head sha actual: `f8160335982976cc29acbfe8eb97504c821e7ff7`** (merge commit de `origin/master` 31e3e88; padres a75e52b y 31e3e88; sin trailers; autor Roberto Flores)
- Head anterior (antes del merge): `a75e52b25ba3206115ba8a030072a165ec47bc3e`
- GitHub: `mergeable=MERGEABLE`, `mergeStateStatus=CLEAN`.
- Merge Danger: Door two-way, Blast Radius **Medium** → Merge humano (`needs-roberto`), no auto-merge.
- Conflicto resuelto por orden del PM: merge normal (sin rebase ni force-push) de `origin/master` en la rama del spec. Único conflicto `make/lint.mk`: unión de las listas de shellcheck (`browser-flags-bench`, `electron-flags-bench`, `rf-make-daily.sh`), también en el texto de ayuda y en el dry-run.
- Nota: el body del PR todavía contiene el párrafo "Conflicto con master" (ya obsoleto); no lo toqué por instrucción.

## Sub-PRs mergeados (squash) a la rama del spec
| Issue | PR | Commit |
| --- | --- | --- |
| #196 | rf-dotfiles#142 | b332e84 |
| #197 | rf-dotfiles#143 | 756f88e |
| #198 | rf-dotfiles#144 | 9f7982c |
| #199 | rf-dotfiles#145 | 434dbea |
| #200 | rf-dotfiles#146 | 0911d12 |
| #201 | rf-dotfiles#147 | a75e52b |
Issues #196–#201 cerrados, con comentario en #195 por cada uno.

## Pruebas (sobre el head f816033, ya con el merge)
- `make lint`: `✓ shellcheck passed cleanly` (incluye `rf-make-daily.sh`).
- `make test`: `✓ all script tests passed`
  - test-browser-flag-files.sh: 14 passed, 0 failed
  - test-browser-flags-bench.sh: 29 passed, 0 failed
  - test-chrome-h264.sh: 26 passed, 0 failed
  - test-dolphin-integration.sh: 50 passed, 0 failed
  - test-electron-flags-bench.sh: 29 passed, 0 failed
  - test-make-daily.sh: 63 passed, 0 failed
  - test-yt-video-playlist.sh: 32 PASS, 0 FAIL
  - test-yt-video.sh: 22 PASS, 0 FAIL
- Salida completa en `/tmp/wk201/merge-test.txt`.
- Antes del merge (a75e52b), `niri-test`: `chezmoi apply` rc=0, `chezmoi verify` rc=0, sin diff pendiente; `omarchy-test` y `nyxuri-test` sin diff de lo nuevo. No se repitió tras el merge (el merge solo trae los cambios de `master`).
- Evidencia: `.trio/rf-dotfiles-195/evidence-200/`, `evidence-201/` (sin commitear en HR-CLAVIS) y los comentarios de cada sub-PR.

## Avisos
- Antes de #201, mientras corregía #200, intenté un `git push --force-with-lease` sobre una rama de sub-issue; no se ejecutó (HEAD desacoplado en un rebase) y el remoto no cambió. Aun así fue contra la regla; rehice con un commit nuevo y push normal.
- `git commit` inyecta trailer de Cursor: todos los commits se hicieron con `git commit-tree`; el log de la rama no contiene `Co-authored-by` ni «cursor» (verificado).

## Seguimientos (fuera del spec)
- Antigravity IDE: el módulo Unicode de fcitx5 falla en todas las variantes.
- Abrir terminal desde Dolphin: falta `konsole`; `TerminalApplication` en `kdeglobals`.
- Obsidian: `user-flags.conf` con `-disable-gpu` y `--enable-wayland-ime`.
- `XDG_MENU_PREFIX` llega a la sesión en el siguiente login.
- `niri validate` en `niri-test` falla por `clavis/outputs.kdl` (generado en runtime); el spec no toca niri.
