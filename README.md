# HR-CLAVIS

Home Repo central del ecosistema **PJ-CLAVIS** (Workstation, Dotfiles, Forkhub, Workflows) y meta-repo del trío PM/WK/RV-HR-CLAVIS: el PM de acá es la única puerta de entrada a todos los repos del proyecto. Hijo de `robert-flo/pj-template` en modo dependencia: acá viven solo `proyecto.yml`, `onboarding.yml`, este README, `AGENTS.md`, `.gitignore`, `./pj`, el `Makefile` de atajos, el workflow `pj-template.yml`, `.trio/` y los extras de `proyecto.yml`.

```bash
make agy-pm     # el de todos los días: git pull + ./pj listo y abre PM-HR-CLAVIS en agy vía mise (make agy-wk / agy-rv; ARGS="…")
./pj listo      # todo de una tras clonar: pull, template, genera, hooks, onboarding (si falta), agy, clonar, check (--abrir arranca el PM)
./pj onboard    # entrevista del PM (onboarding.yml + repos); la 2ª vez muestra los datos y pregunta qué cambiar
./pj clean      # deshace ./pj listo para repetirlo (--onboarding: también el onboarding); no borra clones con trabajo sin subir
./pj            # solo trae pj-template al template_ref y genera .agents/ y .cursor/
./pj check      # esquema de proyecto.yml, idempotencia y prueba de humo
./pj clonar     # clona los repos de proyecto.yml adentro
./pj enlazar    # solo enlaza las fichas en agy (./pj listo ya lo hace)
make            # lista los atajos (agy-pm, agy-wk, agy-rv, listo, onboard, clean, check)
./pj humo       # prueba de humo; con agy instalado arranca PM-HR-CLAVIS sin interacción
code HR-CLAVIS.code-workspace   # o cursor HR-CLAVIS.code-workspace
```

Necesitás `python3`, `gh`, `git` y `make` (`./pj` arma su propio `.pj/venv` con PyYAML y jsonschema). Mapa de repos, punteros y reglas globales: `AGENTS.md`. Compatibilidad Claude Code: `CLAUDE.md`.
