# 🐳 Docker SQL Playground

> Repositorio de práctica personal para aprender Docker + PostgreSQL + Base de Datos.
> Incluye el paso a paso desde cero para poder usar este repositorio en tu máquina.

## Contexto académico
Este repositorio fue desarrollado como parte de las materias **Infraestructura de datos** y **Base de Datos** de la **Tecnicatura en Ciencia de Datos** de la **UNDEC**. El objetivo es tener un entorno reproducible para practicar SQL sin depender de instalaciones locales.

## Requisitos
- Docker Desktop
- PowerShell (o puedes adaptarlo a tu terminal de preferencia)

## Quickstart
```bash
git clone https://github.com/gpelo-data/docker-sql-playground.git
cd docker-sql-playground
cp .env.example .env
docker compose up -d
docker compose exec db psql -U dockerito -d db_chilecito
```
## Estructura del proyecto

```TXT
C:.
│   .env.example        # plantilla; copialo a .env
│   .gitignore
│   docker-compose.yml
│   README.md
│   
└───init
        01-schema.sql
```

> ⚠️ El archivo .env no está en el repo (por seguridad). Se crea al copiar .env.example en el quickstart.

## Cómo conectarse

### Desde un cliente gráfico (DBeaver, pgAdmin, etc.)

| Campo | Valor |
|---|---|
| Host | `localhost` |
| Puerto | `5432` |
| Usuario | `dockerito` |
| Contraseña | `secreto1234` |
| Base de datos | `db_chilecito` |

### Desde la terminal (psql)

```bash
docker compose exec db psql -U dockerito -d db_chilecito
```

> 💡 **Nota**: este contenedor incluye PostGIS. Si vas a usar geometrías, asegurate de que tu cliente soporte tipos espaciales (DBeaver y pgAdmin lo hacen por defecto).



## Versión de Postgres usada
Para esta práctica estaremos usando `postgis/postgis:15-3.4`, que extiende Postgres 15 con soporte para datos geoespaciales (extensión PostGIS).

## Bind Mount
Para generar las tablas cuando levantemos el servicio, creamos la carpeta `./init` que contiene `01-schema.sql` con la creación de las tablas necesarias para el entorno. Al montarse como bind mount, cualquier cambio en el schema se refleja al recrear el contenedor, sin necesidad de reconstruir la imagen.
```YML
volumes:
  - ./init:/docker-entrypoint-initdb.d
```

Las tablas que se crean son:
```sql
authors
books
books_transactions
members
publishers
```


## Cómo resetear el entorno 
```bash
docker compose down -v
docker compose up -d
```