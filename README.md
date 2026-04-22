# 🐳 1. What is Docker? (Beginner Level)

Docker is a **containerization platform**.

It lets you package:

* Your code
* Dependencies
* Runtime
* OS libraries

Into a **container** that runs the same everywhere.

> 💡 “It works on my machine” problem → Docker solves this.

---

# 🖥 2. Why Docker is Important (Real Industry Context)

Imagine you build:

* Django backend
* React frontend
* PostgreSQL database
* Redis
* ML model

Without Docker:

* Each dev installs everything manually
* Different Python versions
* Different node versions
* Deployment issues

With Docker:

* One config file
* One command
* Same environment everywhere

That’s why **every serious backend / AI / DevOps system uses Docker.**

---

# 🧱 3. Core Concepts (Very Important)

## 3.1 Image

Blueprint for container.

Think:

```
Image = Class
Container = Object
```

Example:

```
python:3.11
node:20
postgres:15
```

---

## 3.2 Container

Running instance of an image.

If image is blueprint,
container is the running machine.

---

## 3.3 Dockerfile

Text file that tells Docker:
“How to build the image”

Example:

```dockerfile
FROM python:3.11

WORKDIR /app

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .

CMD ["python", "app.py"]
```

---

## 3.4 Docker Engine

The software that runs Docker.

It includes:

* Docker CLI
* Docker Daemon
* REST API

---

# 🏗 4. How Docker Actually Works (Internals)

Docker uses:

* Linux namespaces
* cgroups
* layered filesystem

Each layer in image = cached layer.

Example:

```
FROM python
RUN pip install django
COPY .
```

If only your code changes,
Docker reuses previous layers → faster builds.

---

# 📦 5. Installing Docker

Go to:

👉 [https://www.docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop)

Install Docker Desktop.

Then verify:

```bash
docker --version
```

Test:

```bash
docker run hello-world
```

If it prints success → you're good.

---

# 🔁 6. Basic Commands (You MUST Know)

### Pull image

```bash
docker pull python:3.11
```

### List images

```bash
docker images
```

### Run container

```bash
docker run python:3.11
```

### Run with interactive shell

```bash
docker run -it python:3.11 bash
```

### List running containers

```bash
docker ps
```

### Stop container

```bash
docker stop <container_id>
```

---

# 🌐 7. Port Mapping (VERY IMPORTANT)

If your app runs on port 8000 inside container:

```bash
docker run -p 8000:8000 myapp
```

Format:

```
-p host_port:container_port
```

---

# 📂 8. Volumes (Data Persistence)

Containers are temporary.

If container stops → data gone.

To persist data:

```bash
docker run -v myvolume:/app/data myapp
```

OR bind mount:

```bash
docker run -v $(pwd):/app myapp
```

---

# 🧠 9. Build Your First Real Project

Let’s Dockerize a simple Flask app.

## Project Structure

```
myapp/
│
├── app.py
├── requirements.txt
└── Dockerfile
```

---

## app.py

```python
from flask import Flask
app = Flask(__name__)

@app.route("/")
def home():
    return "Hello Docker!"

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

---

## requirements.txt

```
flask
```

---

## Dockerfile

```dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

CMD ["python", "app.py"]
```

---

## Build Image

```bash
docker build -t myflaskapp .
```

---

## Run Container

```bash
docker run -p 5000:5000 myflaskapp
```

Open:

```
http://localhost:5000
```

---

# 🔗 10. Docker Compose (Multi-Container Apps)

When you have:

* Backend
* Database
* Redis

You use `docker-compose.yml`

Example:

```yaml
version: '3.9'

services:
  web:
    build: .
    ports:
      - "8000:8000"
    depends_on:
      - db

  db:
    image: postgres:15
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass
```

Run:

```bash
docker compose up --build
```

---

# 🏢 11. Production-Level Knowledge

## Multi-stage builds (optimize image size)

```dockerfile
FROM node:20 AS builder
WORKDIR /app
COPY . .
RUN npm build

FROM nginx:alpine
COPY --from=builder /app/build /usr/share/nginx/html
```

---

## Best Practices

* Use slim images
* Avoid root user
* Use .dockerignore
* Pin versions
* Use multi-stage builds

---

# ☁ 12. Docker in Real Projects (Your Context)

For your:

### AI Job Agent

You’ll likely have:

```
Frontend (React)
Backend (FastAPI/Django)
PostgreSQL
Redis
Celery
```

Docker makes:

* Local dev easy
* Deployment easy
* Scaling easy

---

# 🧨 13. Common Beginner Mistakes

❌ Not exposing correct ports
❌ Forgetting 0.0.0.0
❌ Large image sizes
❌ Not using volumes for DB
❌ Using latest tag blindly

---

# 🚀 14. Advanced Topics (Professional Level)

* Docker networking
* Bridge vs host network
* Docker Swarm
* Kubernetes (next step)
* CI/CD integration
* Private registries
* Image scanning
* Layer caching optimization

---

# 🧭 15. Learning Roadmap (For You)

Since you build AI systems:

### Phase 1

✔ Basic commands
✔ Dockerfile
✔ Port mapping

### Phase 2

✔ Docker Compose
✔ PostgreSQL container
✔ Redis

### Phase 3

✔ Celery workers
✔ Production build
✔ Nginx reverse proxy

### Phase 4

✔ Deploy on AWS / GCP
✔ Kubernetes

---

# 🧠 Mental Model Summary

Docker = Standardized Execution Environment

It ensures:

* Consistency
* Portability
* Isolation
* Scalability

