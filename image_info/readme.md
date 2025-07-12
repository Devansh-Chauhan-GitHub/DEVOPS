# 📘 Docker Base Image Cheat Sheet for Common Technologies

A handy reference for selecting the correct Docker base image depending on your tech stack or project type.

---

## 📦 Frontend Frameworks

| Project Type               | Docker Image Example      | Notes                                                                 |
|----------------------------|---------------------------|-----------------------------------------------------------------------|
| **React / Angular / Vue.js** | `node:<version>`          | Used to install dependencies and run dev servers. Use Alpine variant (`node:18-alpine`) for smaller images. |
| **Static HTML/CSS/JS**     | `nginx:alpine`            | Use for serving static files. Copy build output to `/usr/share/nginx/html`. |

---

## 🧠 Backend Frameworks

| Language / Framework        | Docker Image Example                  | Notes                                                                 |
|-----------------------------|---------------------------------------|-----------------------------------------------------------------------|
| **Node.js (Express, NestJS)** | `node:<version>`                      | Use `node:18-alpine` for smaller, faster builds.                      |
| **Python (Flask, Django)**   | `python:<version>`                    | Common options: `python:3.10-slim`, `python:3.10-alpine`.             |
| **Java (Spring Boot)**       | `openjdk:<version>`                   | e.g., `openjdk:17-jdk-slim`. Combine with Maven/Gradle.              |
| **PHP (Laravel, Symfony)**   | `php:<version>-apache` or `php:<version>-fpm` | Use Apache for built-in server; FPM with NGINX.             |
| **Ruby on Rails**            | `ruby:<version>`                      | e.g., `ruby:3.2-alpine`. May need Node & Yarn too.                   |
| **Go (Golang)**              | `golang:<version>`                    | Use for compiling; deploy with `scratch` or `alpine` base.           |
| **Rust**                     | `rust:<version>`                      | Often used with `debian:bullseye-slim` in multi-stage builds.        |
| **.NET Core / ASP.NET**      | `mcr.microsoft.com/dotnet/sdk`, `runtime` | SDK for build, Runtime for deploy. Use multi-stage builds.     |

---

## 🛢️ Databases

| Database Type     | Docker Image        | Notes                                       |
|-------------------|---------------------|---------------------------------------------|
| **MySQL**         | `mysql:<version>`   | Set `MYSQL_ROOT_PASSWORD`, etc.             |
| **PostgreSQL**    | `postgres:<version>`| Similar setup as MySQL.                     |
| **MongoDB**       | `mongo:<version>`   | Simple and easy for development use.        |
| **Redis**         | `redis:<version>`   | In-memory cache, no config needed.          |
| **MariaDB**       | `mariadb:<version>` | MySQL-compatible drop-in replacement.       |

---

## 📤 Web Servers & Reverse Proxies

| Service         | Docker Image     | Notes                                     |
|------------------|------------------|-------------------------------------------|
| **NGINX**        | `nginx:<version>`| Great for serving static files or reverse proxying. |
| **Apache HTTPD** | `httpd:<version>`| Traditional web server, widely supported. |

---

## 🔧 CI/CD & Tools

| Tool            | Docker Image             | Notes                                                  |
|------------------|--------------------------|--------------------------------------------------------|
| **Jenkins**      | `jenkins/jenkins:lts`    | Extendable with plugins and Docker inside Jenkins.     |
| **GitLab Runner**| `gitlab/gitlab-runner`   | Used in GitLab pipelines for CI/CD.                    |
| **SonarQube**    | `sonarqube:<version>`    | Static code analysis and code quality tracking.        |
| **Trivy**        | `aquasec/trivy`          | Image and code security scanning.                      |
| **Nexus**        | `sonatype/nexus3`        | Artifact repository (Maven, npm, PyPI, Docker).        |
| **Prometheus**   | `prom/prometheus`        | System and service monitoring.                         |
| **Grafana**      | `grafana/grafana`        | Visualization and dashboards for Prometheus data.      |

---

## 🖥️ Build Tools

| Tool             | Docker Image        | Notes                                                       |
|------------------|---------------------|-------------------------------------------------------------|
| **Maven (Java)** | `maven:<version>`   | Use with OpenJDK in multi-stage builds.                     |
| **Gradle (Java)**| `gradle:<version>`  | Java/Kotlin build automation tool.                          |
| **Yarn / npm**   | `node:<version>`    | Node image includes npm; Yarn can be installed on top.      |
| **Composer (PHP)**| `composer:<version>`| Dependency manager for PHP.                                |

---

## 🧪 Testing Tools

| Tool / Framework   | Docker Image                  | Notes                                  |
|--------------------|-------------------------------|----------------------------------------|
| **Selenium**        | `selenium/standalone-chrome` | For end-to-end browser testing.        |
| **Cypress**         | `cypress/included:<version>` | Headless browser E2E tests.            |
| **Postman CLI**     | `postman/newman`             | Run Postman test suites in CLI.        |

---

## ☁️ Cloud CLIs & Utilities

| CLI Tool         | Docker Image                      | Notes                                 |
|------------------|-----------------------------------|---------------------------------------|
| **AWS CLI**      | `amazon/aws-cli`                  | Used in scripting or automation.      |
| **Azure CLI**    | `mcr.microsoft.com/azure-cli`     | For Azure resource management.        |
| **Terraform**    | `hashicorp/terraform`             | Infrastructure as Code (IaC) tool.    |
| **Ansible**      | `ansible/ansible`                 | Configuration management tool.        |

---

### 🛠️ Example Multi-Stage Build: Angular + NGINX

```dockerfile
# Stage 1: Build Angular App
FROM node:18-alpine as builder
WORKDIR /app
COPY . .
RUN npm ci && npm run build --prod

# Stage 2: Serve with NGINX
FROM nginx:alpine
COPY --from=builder /app/dist/your-app-name /usr/share/nginx/html

