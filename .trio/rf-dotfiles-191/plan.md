# Plan — Spec #191: Makefile de rf-dotfiles útil en el día a día de chezmoi

- **Spec:** [robert-flo/HR-CLAVIS#191](https://github.com/robert-flo/HR-CLAVIS/issues/191)
- **Repo de código:** `robert-flo/rf-dotfiles`
- **Base de la rama del spec:** `origin/191-chezmoi-make-diario` (`588939fbdc237f75434099e29deaf79d6af25d62`, igual que `origin/master`)
- **Rama del spec:** `191-makefile-util-en-el-dia`
- **PR final:** `191-makefile-util-en-el-dia` → `191-chezmoi-make-diario` en `robert-flo/rf-dotfiles`, con body `/pr` que referencia `robert-flo/HR-CLAVIS#191`
- **Worktrees:** `/z-home-repos/HR-CLAVIS/repos/rf-dotfiles` (bare en `/z-home-repos/HR-CLAVIS/.bares/rf-dotfiles.git`). No trabajar en el clon `/z-home-repos/HR-CLAVIS/rf-dotfiles`.

## Qué pide el spec

Cinco targets de uso diario, siempre contra la copia que chezmoi lee (`chezmoi source-path`), con el perfil que ya resuelve el usuario (`CHEZMOI_PROFILE` si está; si no, `niri` y usuarios clavis → `clavis`):

- `morning`: solo lectura (puede `git fetch`). Commit de esa copia contra `origin/master`, perfil, si `sudo -n` alcanza, archivos del home que un apply pisaría, y scripts de sistema que se volverían a correr.
- `lose`: solo esos archivos del home. Sale distinto de cero si la lista no está vacía.
- `pull`: `git pull --rebase --autostash` de esa copia desde `origin/master` y, si el clon invocador es otro directorio, también ese clon. Si las dos rutas son la misma, una sola vez. Si el rebase no puede, para. No aplica, no hace push ni commit.
- `safe`: aplica el home solo si `lose` está vacío. Si hay algo local, se detiene, lo imprime y no escribe el home.
- `root`: corre solo la instalación en la raíz (herramientas, hook, paquetes) vía los scripts que ya usan su propio `sudo`. No es `sudo chezmoi` y no aplica el home. Si no hay `sudo -n`, lo dice y no finge que la raíz quedó instalada.

`status` y `diff` usan la misma copia de producción, no el clon desde el que se invocó `make`. `DRY_RUN=1` muestra comandos sin pull ni apply. Los targets viejos de paquetes, lint, doctor y verify siguen. Doctor no se recicla como tablero.

Fuera de alcance: respaldos, apply de un solo archivo, matriz de los tres perfiles, paquetes fuera del manifiesto, cambiar plantillas chezmoi / perfil `clavis` / scripts `yt-*`, auto-merge con cambios locales, y `sudo chezmoi` como root.

Door previsto del PR final: two-way. Blast Radius: Low. Solo cambia el Makefile de rf-dotfiles.

## Orden de sub-issues (DAG)

1. **[#192](https://github.com/robert-flo/HR-CLAVIS/issues/192)** — `rf-dotfiles: morning, lose y pull contra la copia de chezmoi` (`ready-for-agent`, sin bloqueos)
   - `make morning`, `make lose`, `make pull`, y que `make status` / `make diff` comparen el home contra la copia de producción.
   - **Criterios:**
     - `make morning` contra un fixture dice el commit local, si va atrás de `origin/master`, el perfil resuelto y si hay `sudo -n`. Prueba: script de `make test` con git temporal y chezmoi falso.
     - `make lose` lista solo los archivos del home que se perderían y sale distinto de cero cuando la lista no está vacía. Con status falso vacío, no imprime archivos.
     - `make pull` deja la copia de producción (y el otro clon, si difiere) en el commit del remoto local de prueba, y no invoca apply. `DRY_RUN=1` no mueve el git.
     - `make status` y `make diff` usan la copia de producción, no el directorio de invocación cuando son distintos. El test lo demuestra con dos directorios.
   - El WK abre el sub-PR hacia `191-makefile-util-en-el-dia`, corre `/code-review` y lo mergea él (CI verde). RV no entra en este sub-PR.

2. **[#193](https://github.com/robert-flo/HR-CLAVIS/issues/193)** — `rf-dotfiles: safe aplica el home solo si no hay nada que perder, y root solo la raíz` (`ready-for-agent`, bloqueado por #192)
   - `make safe` y `make root`.
   - **Criterios:**
     - Con chezmoi falso que reporta archivos que perder, `make safe` sale distinto de cero, imprime esa lista y no llama apply. El script de `make test` lo demuestra.
     - Con status vacío, `make safe` sí aplica, con el perfil resuelto y la copia de producción como source.
     - `make root` dispara los scripts de sistema y no aplica el árbol del home. `DRY_RUN=1` solo imprime.
     - Si `sudo -n` no está, `make root` lo dice y no pretende que la instalación de la raíz ocurrió.
   - Mismo circuito: sub-PR a la rama del spec, `/code-review`, merge del WK.

## Cierre

Cuando #192 y #193 estén en `191-makefile-util-en-el-dia`, el WK abre el PR final hacia `191-chezmoi-make-diario` (`gh pr create -R robert-flo/rf-dotfiles --base 191-chezmoi-make-diario`, draft según sus reglas, body `/pr` referenciando #191) y escribe `wk.md` con URL, head sha y pruebas. El RV revisa ese PR. El PM aplica el Merge Gate sobre el sha que nombre el RV.
