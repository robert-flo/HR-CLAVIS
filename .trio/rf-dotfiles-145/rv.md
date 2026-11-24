# Revisión del RV — robert-flo/HR-CLAVIS#145

- **Spec:** [robert-flo/HR-CLAVIS#145](https://github.com/robert-flo/HR-CLAVIS/issues/145) — `spec: migrar utilidades de pj-domina-rf-tools hacia rf-dotfiles`
- **Repo destino:** `robert-flo/rf-dotfiles`
- **PR Final:** [robert-flo/rf-dotfiles#113](https://github.com/robert-flo/rf-dotfiles/pull/113)
- **Head SHA:** `219bbefa6f23ea1092007da56c3fb523213a66dc`
- **Rama base:** `master`
- **Rama del PR:** `145-migrar-tools-a-rf-dotfiles`

---

## 1. Validación de Reglas y Convenciones

- [x] **Rama base correcta:** PR apunta hacia `master` en `robert-flo/rf-dotfiles`.
- [x] **Firma y autoría de commits:** Autor verificado `Roberto Flores <25ASAB015@ujmd.edu.sv>`. Cero menciones a agentes, herramientas de IA o trailers `Co-authored-by`.
- [x] **Historial limpio:** Tres commits lineales correspondientes a los sub-issues (#146, #147, #148) sin force-pushes.
- [x] **Ausencia de secretos:** Ninguna clave, credencial o token incluido en el diff.
- [x] **Chezmoi SSoT y portabilidad:**
  - Cero rutas de usuario hardcodeadas (`/home/niri`, `/home/rollo`, etc.).
  - Plantillas usan variables declarativas (`{{ .chezmoi.homeDir }}`, `{{ .chezmoi.sourceDir }}`).
  - Resoluciones dinámicas de librerías (`brief-common.sh`) relativas y con fallback XDG.
- [x] **Gobernanza de `.chezmoiignore.tmpl`:** Las utilidades migradas a `.local/bin` son herramientas comunes de usuario y no interfieren con perfiles aislados.

---

## 2. Pruebas Técnicas en Worktree Efímero (`rv-113`)

1. **Sintaxis de Shell y Linters:**
   - `bash -n` ejecutado en los 7 scripts y plantillas: 100% limpio.
   - `shellcheck` ejecutado sobre la suite completa: 0 advertencias / 0 errores.
2. **Compilación Go:**
   - `go vet ./...` y `go build` en `scripts/brief-table`: compilación exitosa. Flags y dependencias verificadas (`go.mod`, `go.sum`).
   - Hook Chezmoi `run_onchange_after_install-brief-table.sh.tmpl` implementa compilación idempotente basada en hash SHA256 de `main.go`.
3. **Units de Systemd:**
   - Permisos en git `100644`.
   - `systemd-analyze verify` ejecutado sobre `brief.service` y `brief.timer`: 100% limpio, sin errores sintácticos ni dependencias faltantes.
4. **Verificación de Enlaces Simbólicos:**
   - `dot_local/bin/symlink_agent` -> `cursor-agent`
   - `dot_local/bin/symlink_wgup` -> `gup`
5. **Verificación en Vivo en la Estación:**
   - `gup -h` y `wgup -h`: operativos con lectura de allowlist en `~/.config/clavis/gup.allow` y `~/.config/robertos/gup.allow`.
   - `brief -h`: operativo.
   - `brief-table -h`: binario funcional en `~/.local/bin/brief-table`.
   - `statusline.sh`: validado con payload sintético de Cursor; renderizado ANSI correcto.
   - `systemctl --user status brief.timer`: cargado y reconocido por el daemon de usuario.

---

## 3. Merge Gate

- **Door:** `two-way`
- **Blast Radius:** `Low`
- **Elegibilidad de Auto-merge:** Cumple al 100% los criterios de auto-merge squash tras coincidencia de head commit.

---

## 4. Veredicto

APRUEBO 219bbefa6f23ea1092007da56c3fb523213a66dc
