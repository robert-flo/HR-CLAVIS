# Plan — robert-flo/HR-CLAVIS#115

## Ticket
- **Issue:** `robert-flo/HR-CLAVIS#115` (Parent: `#113`)
- **Título:** `yt-video-playlist`: perfil único `<=1080p` y pre-chequeo de subtítulos (anti-429)
- **Repo:** `robert-flo/rf-dotfiles`
- **Base del PR:** `113-yt-video-quality-subtitles`
- **Worktree:** `/z-home-repos/HR-CLAVIS/repos/rf-dotfiles/115-yt-video-playlist`
- **Rama:** `issue/115-yt-video-playlist`

## Referencias en el worktree
- `scripts/yt-common.sh` (helpers compartidos creados en `#114` / PR `#98`)
- `scripts/yt-video` (implementación de referencia para menú de perfil `<=1080p` y pre-chequeo anti-429 de subtítulos)
- `scripts/tests/test-yt-video.sh` (suite de tests con `yt-dlp` mockeado en `PATH`)
- `scripts/yt-video-playlist` (archivo objetivo)

## Qué hay que construir
En `scripts/yt-video-playlist`:
1. **Selección única de perfil de calidad `<=1080p` al inicio:**
   - Preguntar una sola vez al inicio el perfil para toda la playlist:
     - `1) 1080p Estándar/Ligero` (VP9/AV1, sin Premium HLS)
     - `2) 1080p Premium` (máxima calidad `<=1080p`)
     - `3) 720p` (ligero)
   - Soportar flags rápidos `--standard`, `--premium`, `--720p` y bypass con `-f` explícito, manteniendo el tope duro `height<=1080`.
2. **Pre-chequeo y fix de subtítulos (anti-429):**
   - Reutilizar la estrategia anti-429 y el pre-chequeo de subtítulos (de `scripts/yt-common.sh` / `scripts/yt-video`) antes de bajar los videos pesados de la playlist.
   - Avisar explícitamente si un subtítulo no se pudo descargar en vez de ocultarlo tras `--ignore-errors`.

## Criterios de aceptación
- `shellcheck` pasa sin advertencias en `scripts/yt-video-playlist` (y cualquier script/test modificado).
- Tests automatizados de CLI (por ejemplo `scripts/tests/test-yt-video-playlist.sh`, con `yt-dlp` mockeado en `PATH`, sin red) verifican:
  - Prompt único de perfil al inicio (para toda la playlist)
  - Flags `--standard` / `--premium` / `--720p` / `-f`
  - Tope `<=1080p`
  - Manejo de subtítulos ante éxito y fallo `429`
- `chezmoi diff` limpio y sin cambios inesperados.
- PR abierto en `robert-flo/rf-dotfiles` contra la rama `113-yt-video-quality-subtitles`, referenciando `robert-flo/HR-CLAVIS#115` en el body (no `Closes #115`).
- Como es sub-issue de `#113`, con CI verde y `code-review` limpio, el WK mergea el PR a `113-yt-video-quality-subtitles` con `gh pr merge <pr> -R robert-flo/rf-dotfiles --squash --delete-branch --match-head-commit <sha-verde>` y anota el SHA del merge en `wk.md`.
