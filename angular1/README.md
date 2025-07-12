## 📦 Dockerizing an Angular App – Complete Guide

This document explains how the `Dockerfile` was created for this Angular application. Use it to quickly recall why each line exists and how to adapt it for similar projects.

---

### ✅ Dockerfile Used

```Dockerfile
FROM node:18 AS base

WORKDIR /app

COPY /package*.json ./

RUN npm ci

COPY . .

# stage 2
FROM node:18-slim

WORKDIR /app

COPY --from=base /app /app

EXPOSE 4200

RUN npm install -g @angular/cli@13

CMD ["ng", "serve", "--host", "0.0.0.0"]
```

---

### 🔍 Explanation Step-by-Step

#### 1. `FROM node:18 AS base`

- Uses Node 18 as the base build image.
- Named as `base` for multi-stage builds.

#### 2. `WORKDIR /app`

- Sets the working directory inside the container.

#### 3. `COPY /package*.json ./`

- Copies `package.json` and `package-lock.json`.
- Allows Docker to cache dependencies separately.

#### 4. `RUN npm ci`

- Preferred for CI builds.
- Installs exact versions from `package-lock.json`.
- You *can* use `npm install` instead, but it's slower and less deterministic.

#### 5. `COPY . .`

- Adds the full project into the container.

---

### ↺ Why Multi-Stage Build?

- First stage (`node:18`) compiles the app.
- Second stage (`node:18-slim`) only includes the runtime.
- Greatly reduces final image size.

---

### 🔌 EXPOSE 4200

- Angular's default dev server runs on **4200**.
- Makes it externally accessible with Docker port mapping.

---

### 📥 `RUN npm install -g @angular/cli@13`

- Project uses Angular 13 (see `package.json`).
- Angular CLI is needed to run `ng serve`.
- Installed globally in the container.

> ✨ **How did I know?**
>
> Checked `package.json`:
>
> ```json
> "@angular/core": "~13.0.0"
> ```
>
> Matching CLI version: `@angular/cli@13`

---

### ▶️ `CMD ["ng", "serve", "--host", "0.0.0.0"]`

- Starts Angular dev server.
- `--host 0.0.0.0` allows access from outside container.

> 💡 **How did I know to use this?**
>
> Looked at `package.json`:
>
> ```json
> "start": "ng serve"
> ```
>
> Could have used `CMD ["npm", "start"]`, but using `ng serve` gives direct control.

---

### 🧠 How to Know What to Use in CMD/RUN for Any Project

1. **Inspect **``

   - Check `scripts` for start/build/dev commands.
   - Know what tool is used: Angular (ng), React (react-scripts), Node backend (node index.js).

2. **Check framework**

   - Angular: needs CLI, uses `ng serve`.
   - React: uses `npm start`, usually runs on `3000`.
   - Express backend: often `npm start` or `node app.js`.

3. **Look at dev server port**

   - Angular: `4200`
   - React: `3000`
   - Vite: `5173`
   - Match this in Docker `EXPOSE` and `-p` option when running.

---

### 🚀 Build and Run

```bash
docker build -t angular:app .
docker run -d -p 4200:4200 --name angularapp angular:app
```

Open in browser: `http://<your-ec2-ip>:4200`

---

### 🔧 Troubleshooting Tips

- Check `docker logs angularapp` for errors.
- Ensure Angular is serving on `0.0.0.0`, not `localhost`.
- Ensure EC2 security group allows TCP 4200 inbound.

---

### 🕵️‍♂️ Final Notes

- Always check `package.json` and framework.
- Understand which CLI tools are needed.
- Use multi-stage builds for optimized image size.
- Document everything you learn!

---

Created for: **Angular v13 app** | Dockerized on: **Node 18 + Slim**
