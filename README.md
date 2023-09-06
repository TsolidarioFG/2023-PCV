# 2023-PCV
## General Info
```
There are two different projects that compose 2023-PCV volunteer management tool and each one has its README. 
- Backend: Its services are exposed and made with Spring framework and has connection to PostgreSQL database.
- Frontend: Web client made with ReactJS
```
## Run

```
cd backend
mvn sql:execute (only first time to create tables)
mvn spring-boot:run

cd frontend
npm install (only first time to download libraries)
npm start

```

## Production
```

cd backend
Run Dockerfile that internally launches a jar application and exposes it through port 8080


cd frontend
npm run build
```
