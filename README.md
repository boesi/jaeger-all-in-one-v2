# Jaeger v2 – locale Docker container specific for the usage with opentelemetry-maven-extension

There are two configurations:

- storage only in memory, you loose your traces, if you shutdown the container
- persistent locale storage with badger

For convenience there is a minimal configuration for opentelemetry-maven-extenion. Put the folder .mvn in your project folder.

Persistentes Tracing-Backend für das `opentelemetry-maven-extension`.

## Start

```bash
docker compose up -d
```

Jaeger UI: http://localhost:16686

## Stop - keep files (for storage-badger)

```bash
docker compose down
```

## Stop - delete files (for storage-badger)

```bash
docker compose down -v
```

Just use `mvn` like you are used to. You can see your traces under http://localhost:16686

