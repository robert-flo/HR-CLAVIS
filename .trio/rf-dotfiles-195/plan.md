# Plan — robert-flo/HR-CLAVIS#195

Spec: flags de navegadores/Electron/Spotify y Dolphin para `rf-dotfiles` (niri + Intel HD 530).

- Repo de trabajo: `robert-flo/rf-dotfiles` (rama por defecto `master`).
- Rama del spec: `195-flags-dolphin-niri` (ya existe en origin, en `588939f` = `origin/master`).
- Bare: `/z-home-repos/HR-CLAVIS/.bares/rf-dotfiles.git`; worktrees bajo `/z-home-repos/HR-CLAVIS/repos/rf-dotfiles/<n>-<slug>`.
- Cada sub-PR va a `195-flags-dolphin-niri` y cierra su ticket con `Closes robert-flo/HR-CLAVIS#<N>`. Body con `## Summary`, `## Evidence`, `## Merge Danger`. Sin trailers `Co-authored-by`.
- PR final: `195-flags-dolphin-niri` -> `master` (lo abre el WK; lo revisa el RV; lo mergea el PM solo con `APRUEBO <sha>` y Merge Gate).

Prioridad de decisión: estabilidad > CPU/calor/ventilador con video 1080p > suavidad en niri > escritura en español con fcitx5 > arranque y RAM.

## Orden de sub-issues (DAG)

Dos carriles independientes; el WK los toma en este orden secuencial:

| # | Ticket | Bloqueado por | Criterio clave |
| --- | --- | --- | --- |
| 1 | [#196](https://github.com/robert-flo/HR-CLAVIS/issues/196) Chrome estable: evidencia y base de flags | — | Script reutilizable (shellcheck, target make, prueba, idempotente); tabla flag por flag con decisión y evidencia; `chezmoi diff` solo flags de Chrome estable; apply en `niri-test` idempotente; aviso de ventanas en pantalla |
| 2 | [#197](https://github.com/robert-flo/HR-CLAVIS/issues/197) H.264 en YouTube opt-in (Chrome estable) | #196 | Mecanismo elegido con evidencia; CPU antes/después con el script de #196; inactivo = comportamiento actual; activar/desactivar en un paso sin residuos |
| 3 | [#198](https://github.com/robert-flo/HR-CLAVIS/issues/198) Chrome Beta, Chromium, Edge, Brave | #196, #197 | Tabla por navegador con el script de #196; diferencias con razón; decisión sobre extensiones de Omarchy y `--process-per-site`; ningún flag inexistente |
| 4 | [#199](https://github.com/robert-flo/HR-CLAVIS/issues/199) Electron y Spotify | #196 | Tabla por flag; `á é ñ` en VS Code y Antigravity con/sin `--enable-wayland-ime`; CPU Spotify con/sin `--disable-gpu`; verificar que el wrapper real lee cada flag |
| 5 | [#200](https://github.com/robert-flo/HR-CLAVIS/issues/200) Dolphin instalado e integrado | — | Paquetes solo en `.chezmoidata/packages.json`; "Abrir con" poblado; `xdg-mime ... inode/directory` sigue en Nautilus; sin cambios en otros perfiles ni en la variable global de Qt |
| 6 | [#201](https://github.com/robert-flo/HR-CLAVIS/issues/201) Dolphin: preferencias sembradas y apariencia | #200 | Siembra única que no se pisa en el 2.º apply; sin claves HyDE ni 2560x1080; captura + veredicto Kvantum |

Criterio común a todos: `chezmoi diff` solo lo esperado; `chezmoi apply` en `niri-test` en verde e idempotente; sin rutas `/home/<user>` hardcodeadas (`{{ .chezmoi.homeDir }}` en `.tmpl`); paquetes solo en el manifiesto; nuevas rutas fuera de lo común registradas en `.chezmoiignore.tmpl`.

## Fuera de alcance

Zen; cambiar el gestor de archivos por defecto; cambiar la variable global de tema de Qt; otros perfiles; versiones de navegadores o Electron; cualquier cambio fuera de `rf-dotfiles`.
