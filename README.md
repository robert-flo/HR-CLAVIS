# HR-CLAVIS

Home Repo central del ecosistema **PJ-CLAVIS** (Workstation, Dotfiles, Forkhub, Workflows) y meta-repo del trío PM/WK/RV-HR-CLAVIS: el PM de acá es la única puerta de entrada a todos los repos del proyecto. Hijo de `robert-flo/pj-template` en modo dependencia: acá viven solo `proyecto.yml`, este README, `AGENTS.md`, `.gitignore`, `./pj`, el workflow `pj-template.yml`, `.trio/` y los extras de `proyecto.yml`.

```bash
./pj listo      # todo de una tras clonar: pull, template, genera, hooks, agy, clonar, check (--abrir arranca el PM)
./pj            # solo trae pj-template al template_ref y genera .agents/ y .cursor/
./pj check      # esquema de proyecto.yml, idempotencia y prueba de humo
./pj clonar     # clona los repos de proyecto.yml adentro
./pj agy        # una vez por máquina; después: agy --agent PM-HR-CLAVIS
./pj humo       # prueba de humo; con agy instalado arranca PM-HR-CLAVIS sin interacción
code HR-CLAVIS.code-workspace   # o cursor HR-CLAVIS.code-workspace
```

Necesitás `python3`, `gh`, `git` y `make` (`./pj` arma su propio `.pj/venv` con PyYAML y jsonschema). Mapa de repos, punteros y reglas globales: `AGENTS.md`. Compatibilidad Claude Code: `CLAUDE.md`.
