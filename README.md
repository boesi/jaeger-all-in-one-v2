# Jaeger v2 – Local Docker Container for OpenTelemetry Maven Extension

A lightweight, all-in-one Jaeger v2 setup designed to work seamlessly with the [OpenTelemetry Maven Extension](https://github.com/open-telemetry/opentelemetry-java-contrib/blob/main/maven-extension/README.md).

## Prerequisites

- Docker & Docker Compose installed
- Java/Maven project

## Storage Options

### Memory Storage (Default)
Lightweight setup for local development. Traces are lost when the container restarts.

### Persistent Storage (Badger)
Traces are persisted to disk in `./data/`. Use this for longer-running local environments.

To switch configurations:

```bash
cd storage-memory    # or storage-badger
docker compose up -d
```

Jaeger UI: http://localhost:16686

## Usage with OpenTelemetry Maven Extension

The `.mvn/` folder contains minimal configuration files for the OpenTelemetry Maven Extension:

- **extensions.xml** — Configures the OpenTelemetry Maven Extension (v1.55.0-alpha)
- **jvm.config** — Enables trace export to Jaeger via OTLP, disables metrics and logs

### Setup

1. Copy the `.mvn/` folder to your Maven project's root directory
2. Run your build as usual:
   ```bash
   mvn clean install
   ```
3. View your traces at http://localhost:16686

## Ports & Access

- **Jaeger UI**: http://localhost:16686
- **OTLP Receiver (gRPC)**: localhost:4317
- **OTLP Receiver (HTTP)**: localhost:4318

## Stopping

```bash
# Stop and keep data (storage-badger)
docker compose down

# Stop and delete everything (storage-badger)
docker compose down -v
```

## Troubleshooting

- **Traces not appearing**: Verify the `.mvn/` configuration is in your project and the Jaeger container is running (`docker ps`)
- **Port already in use**: Check for conflicting containers with `docker ps` and stop them
- **Storage not persisting**: Ensure you're using the `storage-badger` configuration and not removing volumes with `docker compose down -v`
