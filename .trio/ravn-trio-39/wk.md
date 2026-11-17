# Reporte WK — robert-flo/ravn-trio#39

- **PR:** https://github.com/robert-flo/ravn-trio/pull/41
- **Base:** `37-smoke-spec-herdr`
- **Head SHA (antes del merge):** `d3297df3360992dfdc90020c78742fc663ec2227`
- **Merge SHA (en `37-smoke-spec-herdr`):** `8f8daa6e2bed232b511fa7bd55a8447da2017094`
- **Code review:** limpio (Standards: 0 hallazgos; Spec: 0 hallazgos).
- **CI:** el workflow `.github/workflows/ci.yml` solo dispara en `main`, por lo que no reportó checks en `37-smoke-spec-herdr`.

## Pruebas locales (`/z-home-repos/HR-CLAVIS/repos/ravn-trio/39-smoke-3b`)

### `make lint`

```text
make: Entering directory '/z-home-repos/HR-CLAVIS/repos/ravn-trio/39-smoke-3b'
shellcheck -x pj-trio trio-status runtime/scripts/* runtime/lib/*.sh runtime/helper/*.sh runtime/plugin/* runtime/completion/*.bash tests/*.sh
/bin/sh: line 1: shellcheck: command not found
make: *** [Makefile:37: lint] Error 127
make: Leaving directory '/z-home-repos/HR-CLAVIS/repos/ravn-trio/39-smoke-3b'
```

### `make test`

```text
make: Entering directory '/z-home-repos/HR-CLAVIS/repos/ravn-trio/39-smoke-3b'
bash tests/run.sh
Running test suite in /z-home-repos/HR-CLAVIS/repos/ravn-trio/39-smoke-3b/tests...
=== Running test-base-spec.sh ===
OK base del spec
=== Running test-clonar-proyecto.sh ===
OK clonar proyecto
=== Running test-config-capas.sh ===
OK config por capas
=== Running test-dispatch-package.sh ===
ok test-dispatch-package
=== Running test-espera-agente.sh ===
OK espera de agente
=== Running test-install.sh ===
OK
=== Running test-pj-trio-dry-run.sh ===
{"id":"cli:notification:show","result":{"reason":"shown","shown":true,"type":"notification_show"}}
grep: /tmp/tmp.5lTgv1aYaq/notificado: No such file or directory
FALLO: notify-send no se llamó
make: *** [Makefile:40: test] Error 1
make: Leaving directory '/z-home-repos/HR-CLAVIS/repos/ravn-trio/39-smoke-3b'
```
