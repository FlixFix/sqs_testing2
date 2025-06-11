# Library-Service — Übungsaufgabe

Dieses Projekt ist ein einfaches Java-Backend mit Spring Boot. Es vermittelt den Einsatz verschiedener Testmethoden sowie die Integration in eine automatisierte CI-Pipeline mit GitHub Actions.

---

## Voraussetzungen

Um dieses Projekt zu bauen und auszuführen, werden folgende Werkzeuge benötigt:

| Werkzeug         | Version                 |
|------------------|--------------------------|
| Java             | 17 oder höher            |
| Maven            | 3.8 oder höher           |
| Docker           | 20.10 oder höher         |

---

## Projektstruktur
```
backend/
├── src/main/java/...       # HTTP-, Service-, Speicher-, Entity-Schichten
├── src/test/java/...       # Testklassen

docker-compose.yml          # MySQL-Datenbank für externen Test
pom.xml                     # Maven-Build-Datei
.github/workflows/ci.yml    # GitHub Actions Pipeline
deepsource.toml             # DeepSource-Konfiguration
```



---

## Projekt bauen und lokal ausführen

## 2. Datenbank starten (für externen Test)

```bash
docker-compose up -d
```

Dies startet eine MySQL-Instanz auf `localhost:3306` mit den Zugangsdaten `root/root`.

### 3. Anwendung bauen

```bash
mvn clean package -DskipTests
```

Das erzeugt eine ausführbare JAR-Datei im Verzeichnis `target/`.

---

## Tests ausführen

### Unit-Test (Mockito-basiert)

```bash
mvn test -Dtest=MockitoLibraryTest
```

- Testet den Service isoliert
- Verwendet gemockten Speicherport
- Keine Datenbank notwendig

### Integrationstest (mit Testcontainers)

```bash
mvn test -Dtest=TestContainersLibraryTest
```

- Verwendet eine MySQL-Datenbank in einem Docker-Container
- Docker muss installiert sein

### Test mit externer Datenbank (Docker Compose)

```bash
mvn test -Dtest=ManualLibraryTest
```

- Verbindet sich mit der manuell gestarteten MySQL-Datenbank
- Erwartet eine laufende Datenbank auf `localhost:3306`

---

## Aufgaben für Studierende

### 1. Testmethoden erweitern

Erweitere jede Teststrategie um sinnvolle weitere Testfälle:

- **Unit-Test (Mockito):**
    - Teste Methoden wie `lendBook`, einschließlich fehlerhafter Übergaben
    - Simuliere verschiedene Rückgabewerte und überprüfe die Service-Reaktion
    - Dokumentation: https://site.mockito.org/

- **Integrationstest (Testcontainers):**
    - Teste auch Methoden wie `findByLentTrue`, `findById`, `save`
    - Verifiziere, dass das Mapping zwischen Entity und Business-Objekt korrekt funktioniert
    - Dokumentation: https://testcontainers.com/

- **Externer Datenbank-Test:**
    - Verifiziere das Verhalten der Anwendung bei Zugriff auf bekannte Daten

### 2. GitHub Actions CI-Pipeline erstellen

Erstelle in `.github/workflows/ci.yml` eine CI-Pipeline mit den folgenden fünf Jobs:

| Job | Name                  | Beschreibung                                                         |
|-----|-----------------------|----------------------------------------------------------------------|
| 1   | `build`               | Anwendung mit Maven bauen und Build-Artefakte hochladen              |
| 2   | `run-mockito-tests`   | Artefakte herunterladen und Unit-Tests (ohne Datenbank) ausführen    |
| 3   | `run-db-tests`        | Docker Compose starten und externen Datenbank-Test ausführen         |
| 4   | `run-container-tests` | Integrationstests mit Testcontainers ausführen                        |
| 5   | `deepsource-analysis` | DeepSource Analyse auf dem Quellcode durchführen                     |

Hinweise:

- Jobs 2–4 sollen auf den Build-Artefakten von Job 1 basieren
- In Job 3 muss die Datenbank über `docker-compose up -d` gestartet werden
- Verwende ggf. Caching und Artifact-Sharing zur Optimierung
- Verwende wenn möglich Actions vom Github Actions Marketplace


---

## Lernziele

Nach Abschluss der Übung solltest du in der Lage sein:

- Eine Spring Boot-Anwendung mit 3-Schichtenarchitektur zu verstehen und zu erweitern
- Unterschiedliche Teststrategien anzuwenden (Unit-Test, Integrationstest, DB-abhängiger Test)
- Eine CI-Pipeline mit GitHub Actions in separaten Schritten umzusetzen
- Statische Codeanalyse mit DeepSource durchzuführen


