# backend

## Requirements

- Node 16.
- Java 17 (tested with Eclipse Temurin).
- Maven 3.8+.
- Docker or Podman(currently only for testing)

## BEFORE ALL
### Configurar variables de entorno
Si estas en Windows:
- Click en editar las variables de Entorno del sistema y añadir nuevas variables
```
  EMAIL_ADDRESS, EMAIL_PASSWORD, SIGN_KEY, PCV_DATABASE_PASSWORD, PCV_DATABASE_URL, FRONTEND_URL
  ```

## Configuración de la BD
### Configuración
- En el pom.xml en Data source properties se encuentran las propiedades de conexión y se encuentra un usuario con nombre pcv tanto para la BD de producción como de test
### Producción
- Se necesita una BD desplegada con PostgreSQL. Se probó en un contenedor docker que tiene una imagen de PostgreSQL 15 desplegado en render.com.
- Expira a los 90 dias la versión gratuita (se puede crear otra)
- Para vincularla con el backend tan solo hay que añadir las variables de entorno PCV_DATABASE_PASSWORD para la contraseña de la BD y PCV_DATABASE_URL para la cadena de conexión
### Tests
- Se necesita crear un contenedor local en el puerto localhost:5433 y descomentar del pom.xml <testDataSource.url y <executions
Para crear el contenedor local, en windows. Se recomienda instalar Podman Desktop
```
podman run -d --name pcvtest -e POSTGRES_PASSWORD=pcv -p 5433:5432 docker.io/library/postgres:latest

-If you have a local image
podman run -d --name pcvtest -e POSTGRES_PASSWORD=pcv -p 5433:5432 localhost/my-postgres:image

podman exec -it pcvtest psql -U postgres -c "CREATE USER pcv WITH PASSWORD 'pcv';"
podman exec -it pcvtest psql -U postgres 
CREATE DATABASE pcvtest WITH OWNER pcv;    
GRANT ALL PRIVILEGES ON DATABASE pcvtest TO pcv;
quit
```

## Run

```
mvn sql:execute (only first time to create tables)
mvn spring-boot:run
```

## OPENAPI specification
### Acessible mientras la política cors sea permisiva
```
http://localhost:8080/swagger-ui/index.html?url=v3/api-docs
http://localhost:8080/v3/api-docs (OpenApi JSON format)
If you want to access swagger-ui you need replace "denyAll" for "permitAll" in corsPolicy backend/common/SecurityConfig
```
## Production

```
Option 1 
mvn clean package

Option 2 (RECOMMENDED)
Run Dockerfile through a service provider that runs application and exposes rest services under https on port 8080 (check Dockerfile)
```