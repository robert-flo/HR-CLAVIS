# WK — robert-flo/HR-CLAVIS#114

- **Ticket:** `robert-flo/HR-CLAVIS#114` (sub-issue de `#113`)
- **PR:** https://github.com/robert-flo/rf-dotfiles/pull/98 (base: `113-yt-video-quality-subtitles`)
- **Head SHA:** `0e32223acfa941a4418495192451e029e083a92f`
- **Merge SHA:** `a95f7152b18e68cc8f1b9dbb1e17c1e428dce108` (`MERGED` via `--squash --delete-branch`)

## Archivos modificados / agregados

- `scripts/yt-common.sh` (nuevo helper compartido con `yt_prepare_subtitles` y `yt_select_format`)
- `scripts/yt-video` (pre-chequeo anti-429 de subtítulos + menú manual `<=1080p` sin default)
- `scripts/tests/test-yt-video.sh` (suite CLI sin red con `yt-dlp` mockeado)
- `run_onchange_after_02-install-system-tools.sh.tmpl` (instala `yt-common.sh` en `/usr/local/lib/yt-tools/yt-common.sh`)

## Pruebas verificadas

- `shellcheck` limpio (exit 0) en `scripts/yt-common.sh`, `scripts/yt-video` y `scripts/tests/test-yt-video.sh`.
- `scripts/tests/test-yt-video.sh`: `22 PASS, 0 FAIL` (orden `1080p Premium` → `1080p` `vp9`/`av01`/`h264` → `720p`, exclusión `2160p`, rechazo Enter vacío, bypass `-f`, pre-chequeo exitoso, flujo `429` interactivo/no-interactivo/fallback).
- Prueba real controlada sobre `https://youtu.be/doLKqG8TefU`: descargó subtítulos manuales (`en-eEY6OEpapPo.srt`) sin 429, mostró menú `1..5` (`1080p Premium ~2.41 GiB [id:616]` → `1080p vp9/av01/h264` → `720p vp9`), rechazó Enter vacío y canceló ante EOF sin bajar video.
- Descarga real corta (`https://youtu.be/jNQXAC9IVRw`) + `ffprobe`: confirmó stream de video `240p` (`<=1080p`) y pista `mov_text` (`eng`) embebida en el MP4.
- `chezmoi status`: limpio (`scripts/**` ignorado por `.chezmoiignore.tmpl`; solo dispara `02-install-system-tools.sh`).
- `code-review` (Standards + Spec): hallazgos menores resueltos en `0e32223` y re-verificados.
