<img width="2048" height="2048" alt="image" src="https://github.com/user-attachments/assets/43a32c7f-75eb-4e7b-81e5-882e490e4b9b" />

---
# Docker Multi-stage — GitHub Repo

> **Scenario:** You maintain several microservices (Python API, Node worker, React frontend, Java service). Your images are getting large, CI builds are slow, and production doesn't need build-time tools. You want small, secure runtime images and reproducible builds without maintaining separate build servers.

---

## What is Docker multi-stage build?

Docker multi-stage builds let you use multiple `FROM` stages in one `Dockerfile`. Build artifacts are copied from one stage to the next, so you can run heavy build tools in an intermediate stage and produce a tiny final image that contains only the runtime bits.

Think of it like baking a cake in a full kitchen (build stage) but shipping only the slice (runtime stage).

---

## Why use multi-stage builds?

* **Smaller images** — remove compilers, package caches, and development files from the final image.
* **Faster pulls & deploys** — less data to transfer and store.
* **Reproducibility** — single `Dockerfile` that describes build and runtime steps.
* **Cleaner CI** — one build step replaces build+artifact copying steps.
* **Security** — fewer tools and credentials in runtime image reduce attack surface.

---

## When should you use multi-stage builds?

* When your build requires compilers or heavy tooling (e.g., `pip wheel`, `mvn package`, `npm run build`).
* When you want a small runtime image (Alpine, distroless, or slim images).
* For polyglot repos (mix of Python, Java, Node, React) to keep every service lightweight.
* For CI pipelines where bandwidth, storage, or cold-start times matter.

---

## How to use multi-stage builds — simple patterns

Key ideas:

* Use a `builder` stage that has all build tools.
* Use a lightweight `runtime` stage (e.g., `python:3.11-slim`, `node:18-alpine`, `adoptopenjdk:17-jdk` → `adoptopenjdk:17-jre` or distroless) and `COPY --from=builder` only the artifacts you need.
* Use `--chown` with `COPY` to set file ownership if needed.
* Use build-time arguments (`ARG`) for versions to keep Dockerfiles flexible.

---

## Examples (copy these into each service repo)

### 1) Python (Flask/FastAPI) — build wheels, run slim

```dockerfile
# Builder
FROM python:3.11-slim AS builder
WORKDIR /app
RUN apt-get update && apt-get install -y build-essential gcc --no-install-recommends && rm -rf /var/lib/apt/lists/*
COPY pyproject.toml poetry.lock ./
RUN pip install --upgrade pip && pip install poetry
RUN poetry config virtualenvs.create false && poetry install --no-dev --no-interaction --no-ansi
COPY . .
RUN python -m build -w -o /dist || true

# Final image
FROM python:3.11-slim
WORKDIR /app
COPY --from=builder /dist /dist
# If using wheel: pip install the wheel
RUN pip install --no-cache-dir /dist/*.whl
COPY --from=builder /app/app /app/app
CMD ["gunicorn", "app.main:app", "-w", "4", "-b", "0.0.0.0:8000"]
```

Notes: install build-essential only in builder. Final image has only runtime packages and installed wheel.

### 2) Node (Express worker) — install dev deps, build, use smaller runtime

```dockerfile
# Builder
FROM node:18 AS builder
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm ci
COPY . .
RUN npm run build

# Final
FROM node:18-alpine
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/package.json ./package.json
RUN npm prune --production
CMD ["node", "dist/index.js"]
```

### 3) React (Create React App / Vite) — build static files, serve with nginx

```dockerfile
# Builder
FROM node:18 AS builder
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm ci
COPY . .
RUN npm run build

# Final
FROM nginx:stable-alpine
COPY --from=builder /app/build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

### 4) Java (Maven) — build jar then run minimal JRE

```dockerfile
# Builder
FROM maven:3.9.0-eclipse-temurin-17 AS builder
WORKDIR /app
COPY pom.xml .
COPY src ./src
RUN mvn -B package -DskipTests

# Final
FROM eclipse-temurin:17-jre-jammy
WORKDIR /app
COPY --from=builder /app/target/*.jar app.jar
EXPOSE 8080
CMD ["java", "-jar", "app.jar"]
```

---

## Best practices & tips

* **Keep layers small**: group `RUN` steps and clean caches in the same `RUN` (e.g., `apt-get clean && rm -rf /var/lib/apt/lists/*`).
* **Use `.dockerignore`** to avoid copying source control metadata and node_modules into the image.
* **Use specific base image versions** (no `latest`) for reproducibility.
* **Bake versions as `ARG`** to change base versions without editing Dockerfile.
* **Use non-root user in final image**: `RUN useradd -m appuser && chown -R appuser /app` and `USER appuser`.
* **Enable caching**: copy dependency manifests (`package.json`, `requirements.txt`, `pom.xml`) before copying full source to leverage Docker build cache.
* **Consider multi-arch**: use `docker buildx` when you need cross-platform images.

---

## When NOT to use multi-stage builds

* If your app is tiny and build & runtime are identical (no benefit).
* If you rely on immutable runtime images that include build tools for on-image debugging (rare in prod).

---

## Repo structure suggestion (minimal)

```
/docker-multistage-repo
├─ README.md            # this doc
├─ examples/
│  ├─ python/Dockerfile
│  ├─ node/Dockerfile
│  ├─ react/Dockerfile
│  └─ java/Dockerfile
└─ .gitignore
```

---

## Final note

Multi-stage Dockerfiles are the standard way to get compact, secure, and reproducible images across languages. They respect tradition (one `Dockerfile` per service) while using modern best practices. Start with the example matching your stack and iterate — small images pay off fast.

If you want, I can scaffold the repo files (`examples/*`) and `.gitignore` now. I’ll keep it minimal when you say go.



