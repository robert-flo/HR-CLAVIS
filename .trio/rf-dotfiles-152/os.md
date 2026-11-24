# Reporte Final OS: Ticket #152 (rf-dotfiles)

- **Issue:** [robert-flo/HR-CLAVIS#152](https://github.com/robert-flo/HR-CLAVIS/issues/152)
- **PR:** [robert-flo/rf-dotfiles#115](https://github.com/robert-flo/rf-dotfiles/pull/115)
- **Head:** `0d32cc96643895e10720ed481edc1741a391d2aa`
- **Merge Commit:** `52de88e8feac68cf2132bfcede782baa1072e959`
- **Merge Danger:** `**Door:** two-way` / `**Blast Radius:** Low`

---

## Resumen del Cambio

`executable_statusline-usage.sh` lee `accessToken` de `auth.json` y, si no está, de `secret-tool lookup service cursor-credentials account cursor-user`. El secreto de esta máquina es un objeto JSON: se usa solo `.accessToken`. No se imprime ni se escribe en el caché.

Si el ítem no existe, el error es `sin token`. Un fallo del servicio (aunque el texto diga «not found») queda explícito. Un secreto que no es un objeto con `accessToken` queda como `secreto sin accessToken`.

`executable_statusline.sh`, si el caché tiene `.error` y no tiene `.updated`, muestra `uso: <error>` en vez de `uso: cargando…`.

---

## Evidencia de Pruebas (Proof)

1. `bash -n` y `shellcheck` limpios en los dos scripts.
2. Antes, el caché era `{"has_updated":false,"error":"sin token"}` y `auth.json` solo tiene `storedIn`.
3. Refresco en esta máquina: `{"has_updated":true,"error":null,"h5_cents":3813.5134,"month_pct":51.21240310077519}`. Sin `accessToken`, `refreshToken` ni `Bearer`.
4. JSON sintético por stdin, sin colores: `5h $38.14` y `auto … 55%`. No aparece `uso: cargando…`.
5. Caché sintético `{"error":"sin token","error_at":1}` imprime `uso: sin token`.
6. `secret-tool` falso «item was not found» → `sin token`. «The name org.freedesktop.secrets was not found» se guarda tal cual. stderr vacío → `secret-tool falló`. Secreto crudo → `secreto sin accessToken`. El centinela no queda en el caché.
7. `chezmoi apply` de los dos scripts; quedan iguales al worktree.
8. `statusCheckRollup` vacío (el repo no tiene workflows), PR `MERGEABLE` / `CLEAN`.
9. Clon central `/z/rf-dotfiles` en `52de88e`.

---

## Merge Gate y Cierre

- Prueba real, code-review sin hallazgo pendiente, sin CI en rojo ni pendiente, rama mergeable, Door two-way y Blast Radius Low.
- Auto-merge squash a `master`. Issue cerrado y sin etiquetas de triage.
- Rama `issue/152-statusline-secret` borrada en el remoto.
