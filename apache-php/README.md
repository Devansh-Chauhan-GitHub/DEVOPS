## 🐘 Dockerizing Apache + PHP App – Documentation

This README explains how the Dockerfile and Docker Compose file were created for an Apache + PHP project. Use this to understand the logic behind each instruction.

---

### ✅ Dockerfile Used

```Dockerfile
FROM php:8.3-apache

WORKDIR /app

COPY . /var/www/html

EXPOSE 80
```

### 📦 Docker Compose File

```yaml
services:
  php:
    build:
      context: .
    ports:
      - 80:80
```

---

### 🔍 Step-by-Step Explanation

#### 1. `FROM php:8.3-apache`

- Uses the official PHP image with Apache pre-installed.
- Great for running PHP apps directly with Apache web server.

#### 2. `WORKDIR /app`

- Sets working directory inside the container for any future RUN/COPY instructions.
- Doesn't affect how Apache serves files.

#### 3. `COPY . /var/www/html`

- Copies all your project files (including `index.php`) into Apache’s default root directory.
- Apache serves files from `/var/www/html`.

#### 4. `EXPOSE 80`

- Exposes port 80 so Apache can serve the app over HTTP.
- Matches the default Apache configuration.

---

### 📄 Why No `CMD` Is Written

> **Why no **``** in the Dockerfile?**
>
> The base image `php:8.3-apache` already defines a default `CMD`:
>
> ```Dockerfile
> CMD ["apache2-foreground"]
> ```
>
> This command starts Apache in the foreground so the container doesn't exit. It's the exact behavior we want, so there's no need to redefine it.

#### ✅ What If You Did Want to Write It?

```Dockerfile
CMD ["apache2-foreground"]
```

Or (not recommended):

```Dockerfile
CMD apache2-foreground
```

Apache must run in the foreground in Docker to keep the container alive.

---

### 🚀 Build and Run

```bash
docker build -t php:latest .
docker run -d -p 80:80 php:latest
```

Or with Docker Compose:

```bash
docker compose up -d
```

Open in browser: `http://<your-ec2-ip>`

---

### 🧠 Final Notes

- Apache serves files from `/var/www/html`.
- You must have an `index.php` or `index.html` inside that folder.
- Use the base image `php:<version>-apache` to save time—Apache is pre-configured.
- Always check Apache logs via `docker logs <container-name>` if something doesn't work.

---

Created for: **Apache + PHP App** | Based on: **php:8.3-apache**

 
