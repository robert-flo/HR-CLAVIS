# Plan — robert-flo/HR-CLAVIS#114

- **Ticket:** `robert-flo/HR-CLAVIS#114` (sub-issue de `#113`)
- **Repo de trabajo:** `robert-flo/rf-dotfiles`
- **Worktree:** `/z-home-repos/HR-CLAVIS/repos/rf-dotfiles/114-yt-video-subs-menu`
- **Rama:** `issue/114-yt-video-subs-menu`
- **Base:** `origin/113-yt-video-quality-subtitles` (PR contra `113-yt-video-quality-subtitles`)

## Qué hay que construir

En `scripts/yt-video` (extrayendo un helper compartido si conviene reutilizarlo en `scripts/yt-video-playlist` e instalándolo vía `run_onchange_after_02-install-system-tools.sh.tmpl`):

1. **Pre-chequeo y fix de subtítulos (anti-429):**
   - Antes de descargar el video pesado, verificar/intentar descargar los subtítulos pedidos (`en` por defecto, o los de `--sub-langs`) aplicando mitigación contra `HTTP Error 429: Too Many Requests` (reintentos/pausas, estrategia de cliente o fallback con `--cookies-from-browser` como ya hace `scripts/yt-x`).
   - Informar claramente al usuario si el subtítulo se descargó o falló; si falla, pedir confirmación explícita antes de bajar el video (y en modo no interactivo, avisar explícitamente en vez de ocultarlo tras `--ignore-errors`).

2. **Menú manual obligatorio de calidad `<=1080p`:**
   - Antes de descargar, consultar los formatos disponibles y mostrar un menú numérico ordenado así:
     - `1080p Premium` (HLS `m3u8`) primero si está disponible, con peso estimado.
     - Demás variantes `1080p` disponibles (`vp9`, `av01`, `h264`) con codec y peso estimado.
     - Mejor variante `720p` disponible como opción final.
     - Sin opción por defecto al presionar Enter (exigir elección manual válida).
     - Tope duro `height<=1080` (nunca `>1080p`). Bypass del menú si el usuario pasa `-f` explícito o no hay terminal interactiva.

## Criterios de aceptación (todo verificado en el PR)

- `shellcheck` limpio sin advertencias en `scripts/yt-video` y cualquier helper nuevo o modificado.
- Tests automatizados de CLI (con `yt-dlp` mockeado en `PATH`, sin red) verificando: orden del menú (Premium → estándar `1080p` → `720p`), rechazo de Enter vacío, bypass con `-f`, pre-chequeo de subtítulos exitoso y flujo ante `429` con confirmación.
- Prueba real controlada sobre `https://youtu.be/doLKqG8TefU` (sin descargar los 1.5 GB de video) demostrando en el PR el menú real con pesos y la resolución/estado de subtítulos en inglés.
- Descarga real de un video corto en inglés con `ffprobe` adjunto en el PR confirmando resolución `<=1080p` y pista de subtítulos en inglés embebida en el MP4.
- `chezmoi diff` limpio y sin cambios inesperados; PR abierto contra la rama `113-yt-video-quality-subtitles` en `robert-flo/rf-dotfiles`.
- En el body del PR referenciar `robert-flo/HR-CLAVIS#114` (sin `Closes #114`, ya que es un sub-issue).
- Al ser sub-issue: con CI verde y `code-review` limpio, el WK mergea su PR a `113-yt-video-quality-subtitles` con `gh pr merge <pr> -R robert-flo/rf-dotfiles --squash --delete-branch --match-head-commit <sha-verde>` y anota el SHA del merge en `wk.md`.
