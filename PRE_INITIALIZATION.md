# Pre-initialization

## Objetivo

Preparar el esqueleto inicial del monorepo, Docker Compose y PostgreSQL antes de desarrollar las APIs.

En esta etapa no se implementa lógica de negocio todavía.

---

## 1. Crear estructura inicial

```bash
mkdir pattern-lab-monorepo
cd pattern-lab-monorepo

mkdir -p apps
mkdir -p packages/contracts
mkdir -p infra/postgres/init
mkdir -p docs
```

Estructura esperada:

```txt
pattern-lab-monorepo/
├── apps/
├── packages/
│   └── contracts/
├── infra/
│   └── postgres/
│       └── init/
└── docs/
```

---

## 2. Docker Compose inicial

Archivo:

```txt
infra/docker-compose.yml
```

Contenido sugerido:

```yaml
services:
  postgres:
    image: postgres:16
    container_name: pattern-lab-postgres
    restart: unless-stopped
    environment:
      POSTGRES_USER: dev
      POSTGRES_PASSWORD: dev
      POSTGRES_DB: postgres
    ports:
      - "5432:5432"
    volumes:
      - pattern_lab_postgres_data:/var/lib/postgresql/data
      - ./postgres/init:/docker-entrypoint-initdb.d
    networks:
      - pattern-lab-network

  pgadmin:
    image: dpage/pgadmin4:latest
    container_name: pattern-lab-pgadmin
    restart: unless-stopped
    environment:
      PGADMIN_DEFAULT_EMAIL: admin@patternlab.local
      PGADMIN_DEFAULT_PASSWORD: admin
    ports:
      - "5050:80"
    depends_on:
      - postgres
    networks:
      - pattern-lab-network

volumes:
  pattern_lab_postgres_data:

networks:
  pattern-lab-network:
    driver: bridge
```

---

## 3. Script de creación de bases

Archivo:

```txt
infra/postgres/init/001-create-databases.sql
```

Contenido:

```sql
CREATE DATABASE parking_db;
CREATE DATABASE billing_db;
CREATE DATABASE hospital_db;
CREATE DATABASE logistics_db;
```

Importante:

Este script corre solamente la primera vez que se crea el volumen de PostgreSQL.

Si ya existe el volumen y cambiás este archivo, PostgreSQL no lo vuelve a ejecutar automáticamente.

Para reiniciar desde cero:

```bash
docker compose -f infra/docker-compose.yml down -v
docker compose -f infra/docker-compose.yml up -d
```

---

## 4. Levantar infraestructura

Desde la raíz del proyecto:

```bash
docker compose -f infra/docker-compose.yml up -d
```

Ver contenedores:

```bash
docker ps
```

Entrar a PostgreSQL:

```bash
docker exec -it pattern-lab-postgres psql -U dev -d postgres
```

Listar bases:

```sql
\l
```

---

## 5. Conexiones locales

PostgreSQL:

```txt
Host: localhost
Port: 5432
User: dev
Password: dev
```

Bases:

```txt
parking_db
billing_db
hospital_db
logistics_db
```

pgAdmin:

```txt
URL: http://localhost:5050
Email: admin@patternlab.local
Password: admin
```

---

## 6. Variables de entorno sugeridas por API

### Parking API

```env
DATABASE_URL=postgresql://dev:dev@localhost:5432/parking_db
PORT=3001
```

### Billing API

```env
ConnectionStrings__DefaultConnection=Host=localhost;Port=5432;Database=billing_db;Username=dev;Password=dev
ASPNETCORE_URLS=http://localhost:5001
```

### Hospital API

```env
DATABASE_URL=postgresql://dev:dev@localhost:5432/hospital_db
PORT=8001
```

### Logistics API

```env
DATABASE_URL=postgres://dev:dev@localhost:5432/logistics_db?sslmode=disable
PORT=9001
```

---

## 7. Cuándo agregar Redis

No agregar Redis al inicio salvo que la API lo necesite.

Redis tendría sentido para:

- cache de disponibilidad
- locks
- rate limiting
- sesiones
- pub/sub simple

Fase recomendada para Redis: después de terminar la primera API.
