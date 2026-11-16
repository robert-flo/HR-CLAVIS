# HR-CLAVIS

Home Repo central del ecosistema **PJ-CLAVIS** (Workstation, Dotfiles, Forkhub, Workflows). También es el meta-repo del trío PM/WK/RV-HR-CLAVIS (hijo de `robert-flo/pj-template`, modo dependencia).

```bash
./pj listo      # todo de una: pull, template, genera, hooks, agy, clonar, check
./pj            # solo trae pj-template al template_ref y genera .agents/ y .cursor/
./pj check      # esquema de proyecto.yml, idempotencia y prueba de humo
./pj agy        # una vez por máquina; después: agy --agent PM-HR-CLAVIS
```

- Punteros y reglas globales para agentes: [`AGENTS.md`](AGENTS.md)
- Compatibilidad Claude Code: [`CLAUDE.md`](CLAUDE.md)
- Datos del trío: [`proyecto.yml`](proyecto.yml); handoffs de `pj-trio`: [`.trio/`](.trio/)
- Skills compartidas: `.agents/skills/` (las genera `./pj` desde pj-template)
