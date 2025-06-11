# Dokumentation für SQS Testing

Dieses Projekt zeigt euch, wie man API-Dokumentation mit folgenden Tools erstellt und veröffentlicht:
- **Sphinx** (zum Generieren der Dokumentation)
- **Markdown** (mit MyST für den Inhalt)
- **OpenAPI** (automatisch generierte API-Spezifikation)
- **Read the Docs** (für Hosting)
- **Furo-Theme** (für ein modernes, sauberes Layout)

Es integriert eine OpenAPI-YAML-Datei in die generierte Dokumentation, die automatisch bei [Read the Docs](https://sqs-testing2.readthedocs.io/en/latest/) gebaut und gehostet wird.

---

## Dateiübersicht

### `.readthedocs.yml`

Konfiguriert, wie Read the Docs das Projekt baut.

```yaml
version: 2

build:
  os: ubuntu-24.04
  tools:
    python: "3.13"

sphinx:
  configuration: docs/conf.py

python:
  install:
    - requirements: docs/requirements.txt
```

📌 Wichtig:
- `sphinx.configuration` zeigt auf die `conf.py` Datei.
- Die Anforderungen werden automatisch installiert.
- Wenn ihr euer Repository von ReadTheDocs verbindet, erstellt ReadTheDocs diese Datei automatisch für euch

---

### `docs/conf.py`

Sphinx-Konfigurationsdatei.

Aktivierte Erweiterungen:

```python
extensions = [
    'sphinx.ext.autodoc',
    'sphinx.ext.viewcode',
    'sphinx_copybutton',
    'myst_parser',
    'sphinxcontrib.openapi'
]
```

- `html_theme = 'furo'`: Modernes Layout
- `root_doc = 'index'`: Einstiegspunkt
- `sphinxcontrib.openapi'`: OpenAPI Rendering Erweiterung

---

### `docs/requirements.txt`

Python-Abhängigkeiten für den Dokumentations-Build:

```
sphinx==6.2.1
furo==2023.3.27
myst-parser==1.0.0
sphinx-copybutton==0.5.2
sphinxcontrib-openapi>=0.6.0
```

Hier sind ähnlich einer package.json Versionsnummern mit boolschen Operationen möglich.

---

### `docs/index.md`

Startpunkt der Dokumentation. Enthält in unserem Beispiel nur das Inhaltsverzeichnis:

```markdown
# SQS Testing Dokumentation

```{toctree}
:maxdepth: 2
:caption: Inhalte:

api.md
```
die TocTree Erweiterung rendert ein Inhaltsverzeichnis, das sich durch viele verschiedene Einstellungen noch weiter personalisieren lässt. Weitere Informationen dazu findet ihr [hier](https://sphinx-rtd-trial.readthedocs.io/en/latest/markup/toctree.html).

---

### `docs/api.rst`

Rendert die OpenAPI-Dokumentation:
```
API Documentation
==================

.. openapi:: openapi.yaml
    :request:
    :group:
    :examples:
```
 
**Erklärung der Optionen:**
- `:request:`: Zeigt Request-Bodies, falls vorhanden.
- `:group:`: Gruppiert nach `tags`, wie sie in der OpenAPI-Datei definiert sind.
- `:examples:`: Zeigt Beispiel-Daten, wenn im YAML hinterlegt.

---

### `docs/openapi.yaml`

Die OpenAPI 3.0-Spezifikation lässt sich beispielsweise automatisch mit der entsprechenden Maven-Dependecy und folgendem Befehl automatisch aus dem Spring Boot-Backend generieren:

```bash
curl http://localhost:8080/v3/api-docs.yaml -o docs/openapi.yaml
```

---

## Repository mit Read the Docs verbinden

1. Repository auf **GitHub**, **GitLab** oder **Bitbucket** pushen.
2. Besuche [https://readthedocs.org/](https://readthedocs.org/) und registriere dich.
3. Wähle **„Import a Project“**.
4. Wähle dein Repository aus.
5. Read the Docs erkennt `.readthedocs.yml` und baut automatisch die Doku.
6. Die Seite ist dann verfügbar unter:
   ```
   https://<dein-projekt>.readthedocs.io/en/latest/
   ```

---

## Lokale Vorschau

```bash
cd docs
pip install -r requirements.txt
make html
open _build/html/index.html
```

---

## Zusammenfassung

| Komponente | Zweck |
|------------|-------|
| `.readthedocs.yml` | Build-Konfiguration |
| `conf.py` | Sphinx-Einstellungen |
| `requirements.txt` | Python-Abhängigkeiten |
| `index.md` | Einstiegspunkt |
| `api.md` | OpenAPI-Dokumentation |
| `openapi.yaml` | Die API-Spezifikation |

---

## Hilfe & Ressourcen

- [Read the Docs Anleitung](https://docs.readthedocs.io/)
- [MyST Markdown-Syntax](https://myst-parser.readthedocs.io/)
- [sphinxcontrib-openapi](https://github.com/sphinx-contrib/openapi)

