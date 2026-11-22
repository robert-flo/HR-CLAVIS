# WK Report — robert-flo/HR-CLAVIS#115

- **PR:** https://github.com/robert-flo/rf-dotfiles/pull/99 (`robert-flo/rf-dotfiles#99`)
- **Base:** `113-yt-video-quality-subtitles`
- **Head branch:** `issue/115-yt-video-playlist` (borrada tras squash-merge)
- **Head SHA:** `4bd543c2d182f810f10a1d74d2cae576b21317b8`
- **Merge SHA:** `86de56347582902ce608c8a55339f05c068f4d75` (squash-merge en `113-yt-video-quality-subtitles`)

## Cambios realizados
- `scripts/yt-video-playlist`:
  - Selección única de perfil `<=1080p` al inicio para toda la playlist (`1) 1080p Estándar/Ligero`, `2) 1080p Premium`, `3) 720p`), sin default en modo interactivo.
  - Flags rápidos `--standard`, `--premium`, `--720p`, `--no-subs` y bypass con `-f`/`--format` explícito, manteniendo el tope duro `height<=1080`.
  - Pre-chequeo de subtítulos por entrada (`--flat-playlist`) reutilizando `yt_prepare_subtitles` de `scripts/yt-common.sh` (estrategia anti-429) antes de bajar videos pesados.
  - Eliminado `--ignore-errors` del flujo por defecto; ante fallo `429` informa el error explícito, lista las URLs afectadas y pide confirmación en modo interactivo o avisa en modo no interactivo.
- `scripts/tests/test-yt-video-playlist.sh`:
  - Suite automatizada sin red con `yt-dlp` mockeado en `PATH`.

## Pruebas verificadas
- `shellcheck scripts/yt-common.sh scripts/yt-video scripts/yt-video-playlist scripts/tests/test-yt-video.sh scripts/tests/test-yt-video-playlist.sh`: **0 advertencias**.
- `bash scripts/tests/test-yt-video-playlist.sh`: **32 PASS, 0 FAIL**.
- `bash scripts/tests/test-yt-video.sh`: **22 PASS, 0 FAIL**.
- `chezmoi execute-template` sobre `run_onchange_after_02-install-system-tools.sh.tmpl`: render limpio con el hash actualizado de `scripts/yt-video-playlist`.
- `code-review` (Standards + Spec en paralelo): hallazgos corregidos y re-verificados antes del merge.
