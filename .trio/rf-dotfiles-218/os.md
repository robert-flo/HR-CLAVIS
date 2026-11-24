# OS · rf-dotfiles-218 (robert-flo/HR-CLAVIS#218)

Estado: **hecho** (auto-merge por Merge Gate).

- PR: https://github.com/robert-flo/rf-dotfiles/pull/156
- Head revisado: `67f6127ba566088cba6e8061ae7935deb6029fc3`
- Merge commit (squash): `6e144392389eae78e93051e5d1a0c6453045d17c`
- Merge Danger: Door: two-way · Blast Radius: Low

## Qué se hizo

`make diff`, `make safe` y `make root` piden `rf-make-daily.sh source --canonical`, igual que `status` y `morning`. El comentario de `rf_source` ya no dice que el resto se queda con el enlace. `make pull` sigue con `source` + `realpath`. `make apply` sigue usando el directorio desde el que se invocó. `lose` no cambia.

## Evidencia

- Antes, `scripts/tests/test-make-daily.sh`: 108 passed, 7 failed. Con chezmoi real, un directorio y un symlink, `make diff` listaba el script como `new file`. `DRY_RUN` de `safe` y `root` pasaba `-S` del enlace.
- Después, la misma suite: 115 passed, 0 failed. `diff` no nombra `probe.sh` ni dice `new file`. `safe` y `root` muestran `-S` de la ruta canónica.
- `make test`: `✓ all script tests passed`. `make lint`: `✓ shellcheck passed cleanly`.
- Repo sin CI (no hay `.github/workflows`; `statusCheckRollup` vacío, igual que el PR #155). `mergeable=MERGEABLE`, `mergeStateStatus=CLEAN`.
- `code-review`: Standards sin violaciones; Spec sin hallazgos.
- Issue 218 cerrado, sin etiquetas de triage.
