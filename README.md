# demo-spring-efrei — Ateliers EFREI (GitHub · Dockerfile · Spring Boot · GitHub Actions)

Projet Spring Boot unique couvrant les **3 ateliers** :

1. **Atelier 1 — GitHub** : projet Spring Boot Maven poussé sur GitHub.
2. **Atelier 2 — Dockerfile + Spring Boot** : API REST `GET /` → `Hello, World!`, image Docker, push Docker Hub.
3. **Atelier 3 — GitHub Actions** : pipeline CI/CD (build + test Maven, puis build & push de l'image Docker sur `push` vers `main`).

## Lancer en local

```bash
./mvnw clean package          # build + tests -> target/demo-0.0.1-SNAPSHOT.jar
./mvnw spring-boot:run        # puis http://localhost:8080  -> Hello, World!
```
> Sous Windows PowerShell : `.\mvnw.cmd` au lieu de `./mvnw`.

## Docker

```bash
docker build -t <user-dockerhub>/springboot-app .
docker run -p 8080:8080 <user-dockerhub>/springboot-app
docker login
docker push <user-dockerhub>/springboot-app:latest
```

## CI/CD (Atelier 3)

`.github/workflows/deploy.yml` se déclenche sur `push`/`pull_request` vers `main` et `develop` :
- **build** : checkout → JDK 17 → `mvnw clean verify` → upload du `.jar`.
- **docker** (uniquement sur `main`) : login Docker Hub → build & push de l'image.

Secrets requis (GitHub → Settings → Secrets and variables → Actions) :
- `DOCKER_USERNAME`
- `DOCKER_PASSWORD` (access token Docker Hub)
