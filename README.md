# Jaeger v2 – lokales Docker Compose Setup

Persistentes Tracing-Backend für das `opentelemetry-maven-extension`.

## Starten

```bash
docker compose up -d
```

Jaeger UI: http://localhost:16686

## Stoppen (Daten bleiben erhalten)

```bash
docker compose down
```

## Stoppen + Daten löschen

```bash
docker compose down -v
```

---

## opentelemetry-maven-extension konfigurieren

### Umgebungsvariablen (vor dem Maven-Aufruf setzen)

```bash
# Pflicht: Exporter aktivieren
export OTEL_TRACES_EXPORTER=otlp

# Optional: Service-Name in der Jaeger UI
export OTEL_SERVICE_NAME=mein-projekt
```

Dann Maven wie gewohnt aufrufen:

```bash
mvn clean verify
```

### Alternativ: Systemproperties

```bash
mvn clean verify \
  -Dotel.traces.exporter=otlp \
  -Dotel.service.name=mein-projekt
```

### In der pom.xml (Extension deklarieren)

```xml
<build>
  <extensions>
    <extension>
      <groupId>io.opentelemetry.contrib</groupId>
      <artifactId>opentelemetry-maven-extension</artifactId>
      <version>1.44.0-alpha</version>
    </extension>
  </extensions>
</build>
```

> **Hinweis:** Die aktuell neueste Version findest du unter
> https://central.sonatype.com/artifact/io.opentelemetry.contrib/opentelemetry-maven-extension

---

## Ports

| Port  | Protokoll   | Verwendung                          |
|-------|-------------|-------------------------------------|
| 4317  | OTLP/gRPC   | Eingang vom Maven Extension         |
| 4318  | OTLP/HTTP   | Alternativer Eingang                |
| 16686 | HTTP        | Jaeger UI                           |
| 16685 | gRPC        | Jaeger Query API                    |
| 13133 | HTTP        | Health Check (`/` gibt 200 zurück)  |

---

## Hinweise zu Jaeger v2

- **Konfiguration nur über `--config`-Datei** – Environment Variables wie
  `SPAN_STORAGE_TYPE` werden von Jaeger v2 ignoriert.
- **Badger TTL:** Traces werden standardmäßig 72 Stunden gespeichert.
  Anpassbar in `jaeger-config.yaml` unter `ttl.spans`.
- **Datenverlust vermeiden:** `docker compose down` ist sicher.
  `docker compose down -v` löscht das Volume unwiderruflich.
- Das Image `jaegertracing/jaeger:2.17.0` ist gepinnt – für Updates
  die neue Version in `docker-compose.yml` eintragen und
  `docker compose pull && docker compose up -d` ausführen.
