# Dockerfile Creation (Backend & Frontend)

## Overview

Created production-ready Dockerfiles for both Backend (Java/Maven) and Frontend (HTML/JavaScript/Node.js) applications using Docker best practices.

## What We Delivered

### Backend Dockerfile

* Multi-stage build for optimized image size.
* Maven used for building the application.
* Lightweight JRE image used for runtime.
* Application exposed on **port 8080**.

Example:

```dockerfile
FROM maven:3.9.9-eclipse-temurin-17 AS build
WORKDIR /app
COPY . .
RUN mvn clean package -DskipTests

FROM eclipse-temurin:17-jre
WORKDIR /app
COPY --from=build /app/target/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java","-jar","app.jar"]
```

Build:

```bash
docker build -t backend-app ./backend
```

Run:

```bash
docker run -d -p 8080:8080 backend-app
```

---

### Frontend Dockerfile

* Node.js used for building the frontend.
* Nginx used for serving static files.
* Application exposed on **port 80**.

Example:

```dockerfile
FROM node:18 AS build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

FROM nginx:alpine
COPY --from=build /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx","-g","daemon off;"]
```

Build:

```bash
docker build -t frontend-app ./frontend
```

Run:

```bash
docker run -d -p 80:80 frontend-app
```

## Standardized Directory Structure

```text
Application/
├── backend/
│   ├── Dockerfile
│   ├── pom.xml
│   └── src/
├── frontend/
│   ├── Dockerfile
│   ├── package.json
│   └── src/
└── Dockerfile-Creation.md
```

## Standardized Configuration

| Component | Port | Runtime  |
| --------- | ---- | -------- |
| Backend   | 8080 | Java JRE |
| Frontend  | 80   | Nginx    |

## Production Features

* Multi-stage builds
* Smaller image size
* Lightweight runtime images
* Standardized port mappings
* Production-ready configuration

## Outcome

* Lightweight, secure, and portable container images.
* Standardized build artifacts across development, testing, and production environments.
* Ready for Jenkins CI/CD and Amazon ECR deployment.
