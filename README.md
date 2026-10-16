# Postgres Service

PostgreSQL local listo para desarrollo, ejecutado con Docker Compose.

Levanta una instancia de **PostgreSQL 17 (Alpine)** con credenciales y puerto predeterminados, persistencia de datos y un healthcheck para saber cuándo está lista.

## Requisitos

- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/) (incluido en Docker Desktop y en las versiones recientes de Docker Engine)

## Uso

Iniciar el servicio:

```bash
docker compose up -d
```

Verificar que esté corriendo y saludable:

```bash
docker compose ps
```

Ver los logs:

```bash
docker compose logs -f postgres
```

Detener el servicio (conserva los datos):

```bash
docker compose down
```

Detener y **borrar** los datos:

```bash
docker compose down -v
```

## Conexión

| Parámetro | Valor          |
| --------- | -------------- |
| Host      | `localhost`    |
| Puerto    | `5432`         |
| Usuario   | `postgres`     |
| Password  | `postgres`     |
| Base de datos | `postgres` |

Cadena de conexión:

```
postgresql://postgres:postgres@localhost:5432/postgres
```

Ejemplo con `psql`:

```bash
psql "postgresql://postgres:postgres@localhost:5432/postgres"
```

O desde dentro del contenedor:

```bash
docker compose exec postgres psql -U postgres -d postgres
```

## Configuración

Todo se define en [`docker-compose.yml`](./docker-compose.yml):

- **Imagen:** `postgres:17-alpine`
- **Contenedor:** `postgres-local`
- **Reinicio:** `unless-stopped`
- **Persistencia:** volumen `postgres_data` montado en `/var/lib/postgresql/data`
- **Healthcheck:** `pg_isready` cada 10s (timeout 5s, 5 reintentos)

> **Aviso:** las credenciales son de ejemplo y están pensadas solo para desarrollo local. No las uses en producción.

Para cambiar usuario, password, base de datos o puerto, edita las variables `POSTGRES_*` y el mapeo de `ports` en `docker-compose.yml`.

## Estructura

```
.
├── docker-compose.yml   # Definición del servicio PostgreSQL
└── README.md
```
