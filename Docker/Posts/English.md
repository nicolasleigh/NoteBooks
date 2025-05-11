# How I Use Docker, Docker Compose, Makefile, and Caddy to Deploy My Full-Stack Blog

## 1. Introduction

In modern web development, setting up a clean, consistent, and efficient development environment is just as important as writing the application code itself. Tools like **Dockerfile**, **Docker Compose**, **Makefile**, and **Caddy** can help you build, run, and deploy your applications with ease—but using them together effectively can be a challenge.

In this post, we’ll walk through how to combine these tools to develop and run a full-stack web app with a **Vue frontend** and a **Go backend**.

## 2. Project Overview

**linze.pro** is my personal blog application that allows me to publish and manage blog posts, while visitors can browse articles, view portfolio projects, and learn more about me. It’s designed as a full-stack application with a focus on clean architecture, multi-language support, and fast performance.

To make development and deployment more efficient, the project integrates the following tools:

* **Dockerfile** – to containerize the backend.
* **Docker Compose** – to orchestrate and manage multi-container services.
* **Makefile** – to define simple commands for building, running, and maintaining the app.
* **Caddy** – to serve static frontend files and reverse proxy API requests to the backend, with HTTPS support.

The project is a full-stack app:

* **Frontend**: Built with **Vue** and **Tailwind CSS**, providing a responsive and user-friendly interface.
* **Backend**: Built with **Golang** using the **chi** router, handling authentication, blog post CRUD operations, analytics, and more.

Here is the project folder structure:

```
linze.pro/
├── Caddyfile
├── Makefile
├── backend/
│   ├── Dockerfile
│   ├── .envrc
├── frontend/
├── compose.yaml
├── .gitignore
└── resources/
```

## 3. Dockerfile

Here’s the `Dockerfile` used to containerize the Go backend for `linze.pro`:

```dockerfile
FROM golang:1.23.8-alpine3.20

WORKDIR /usr/src/app

COPY go.mod go.sum ./

# Uses Alibaba’s Go proxy for faster module downloads inside China
RUN go env -w GOPROXY=https://mirrors.aliyun.com/goproxy/,direct

RUN go mod download

# Install migration tool for database setup
RUN go install -tags 'postgres' github.com/golang-migrate/migrate/v4/cmd/migrate@latest

# links `migrate` to a common path
RUN ln -s /go/bin/linux_amd64/migrate /usr/local/bin/migrate

# Copy the source code
COPY . .

# Builds the application to `/usr/local/bin/app`
RUN go build -o /usr/local/bin/app ./cmd/api

# Expose the port used by the app
EXPOSE 8085

# Run the built binary file
CMD ["app"]
```

## 4. compose.yaml

To orchestrate all the services in the `linze.pro` blog app, I use Docker Compose. Here’s the `compose.yaml` file:

```yaml
name: blog

services:
  postgres:
    restart: always
    image: postgres:16.8-alpine3.20
    container_name: blog-postgres
    environment:
      POSTGRES_DB: blog
      POSTGRES_USER: nicolas
      POSTGRES_PASSWORD_FILE: /run/secrets/db_password
    secrets:
      - db_password
    networks:
      - backend_network
    volumes:
      - postgres_data:/var/lib/postgresql/data
    expose:
      - "5432"

  backend:
    restart: always
    build:
      context: ./backend
      dockerfile: Dockerfile
    container_name: blog-backend
    env_file: "./backend/.envrc"
    environment:
      APP_ENV: production
    networks:
      - backend_network
    depends_on:
      - postgres
    ports:
      - "127.0.0.1:8085:8085"

  redis:
    image: redis:6.2-alpine
    restart: always
    container_name: blog-redis
    networks:
      - backend_network
    command: redis-server --save 60 1 --loglevel warning

secrets:
  db_password:
    file: db_password.txt

volumes:
  postgres_data:

networks:
  backend_network:
    driver: bridge
```

### Services Explained

#### `postgres`

* Runs PostgreSQL in an Alpine-based lightweight container.
* Uses secrets to store the DB password securely.
* Stores persistent data via a named volume.
* Connected to the `backend_network`.

#### `backend`

* Builds the Go backend from the `Dockerfile`.
* Loads environment variables from `.envrc`.
* Depends on `postgres` (ensures Postgres starts first).
* Binds to `127.0.0.1:8085` to limit exposure to the host only.
* Shares the same network as Postgres for inter-container communication.

#### `redis`

* Lightweight in-memory store used for caching or rate-limiting.

## 5. Caddy Setup

[Caddy](https://caddyserver.com/) is a powerful web server that supports automatic HTTPS, reverse proxying, and static file serving with minimal configuration. Here's the `Caddyfile` used in the `linze.pro` project:

```
linze.pro {
	handle /api/* {
		reverse_proxy localhost:8085
	}
	handle {
		root * /home/nicolas/linze.pro/vue-build/dist
		try_files {path} /index.html
		file_server
	}
}

file.linze.pro {
	handle {
		root * /home/nicolas/linze.pro/resources
		header {
			Access-Control-Allow-Origin *
			Access-Control-Allow-Methods "GET, OPTIONS"
			Access-Control-Allow-Headers *
		}
		file_server browse
	}
}
```

### Section Breakdown

#### `linze.pro`

This block configures the main site that serves the frontend and handles API routing.

* `handle /api/*`:
  Requests to paths like `/api/v1/posts` or `/api/v1/auth` are **reverse proxied** to the backend Go server running on `localhost:8085`.

* `handle`:

  All other routes (e.g., `/`, `/posts`) serve static files from the Vue build output.

  * `root`: Points to the Vue build files.

  * `try_files {path} /index.html`: Points to the frontend code entry point.

  * `file_server`: Enables static file serving.

#### `file.linze.pro`

This subdomain serves static resources (images and videos)

* `root`: Points to `/resources` folder on disk.
* `header`: Sets **CORS headers** to allow resources to be accessed from other origins. This is required because my project streams videos using HLS, which involves loading `.m3u8` and `.ts` files via JavaScript. If your videos are plain MP4 files served directly by the browser, this CORS configuration is usually not necessary.
* `file_server browse`: Enables directory listing.

## 6. Makefile

The `Makefile` is used to automate frequent tasks like Docker orchestration, database management, frontend deployment, and remote syncing. Here's how it's structured:

### Variables and Environment

```makefile
include ./backend/.envrc
MIGRATIONS_PATH=./cmd/migrate/migrations
```

- `include ./backend/.envrc`: Loads environment variables, such as database DSN, used by the `backend/migrate/up` target.
- `MIGRATIONS_PATH`: Defines the relative path to the database migration files. Since the `migrate` command is executed inside the backend Docker container, the `.` refers to the root of the backend folder within the container context.

### Resource Deployment

```makefile
.PHONY: res/send
res/send:
	@rsync -rP resources nicolas@106.14.126.186:~/linze.pro
```

Syncs the `resources` folder to the remote server using `rsync`.

- I use `rsync` instead of `scp` because `rsync` is more powerful and reliable. In my experience, `scp` behaves inconsistently—for example, when sending the `dist` folder to the remote server, it sometimes skips creating the directory on the first attempt but then creates it automatically on the second. This unpredictability made me switch to `rsync`, which handles file transfers more consistently and efficiently.

- `-r`: (recursive): Copies directories recursively.

- `-P`: (progress and partial): Shows progress during transfer.

**`.PHONY: res/send`**

- Declares `res/send` as a **phony target**, meaning it's not a real file or directory. Without this, if a file or folder named `res/send` exists, `make` will skip running the command. Marking it as `.PHONY` forces the command to always execute.

**`@` before `rsync`**

- Suppresses the default behavior of `make` printing the command to the terminal. This keeps the output cleaner by hiding the `rsync` command itself—only the `rsync` progress and results will be shown.

### Docker Compose Commands

```makefile
.PHONY: compose/build
compose/build:
	sudo docker compose up --build

.PHONY: compose/up
compose/up:
	sudo docker compose up -d
```

- `compose/build`: Builds images and runs containers interactively.
- `compose/up`: Runs containers in detached mode.

### PostgreSQL DB Utilities

```makefile
.PHONY: backend/createdb
backend/createdb:
	sudo docker exec -it blog-postgres createdb -U nicolas blog

.PHONY: backend/psql
backend/psql:
	sudo docker exec -it blog-postgres psql -U nicolas blog
```

* Runs common Postgres CLI commands inside the Docker container.

### Database Migrations

```makefile
.PHONY: backend/migrate/up
backend/migrate/up:
	sudo docker exec blog-backend migrate -database ${CLOUD_DB_DSN} -path ${MIGRATIONS_PATH} up
```

- Runs database migrations using [golang-migrate](https://github.com/golang-migrate/migrate).
- The `CLOUD_DB_DSN` variable is defined inside the `.envrc` file in the backend folder

### Frontend Build & Deployment

```makefile
.PHONY: frontend/build
frontend/build:
	@cd frontend && npm run build && cd ..

.PHONY: frontend/send
frontend/send:
	@cd frontend && rsync -rP dist nicolas@106.14.126.186:~/linze.pro/vue-build && cd ..

.PHONY: bs
bs: frontend/build frontend/send
	@echo build and send finished!
```

- `frontend/build`: Builds the Vue app.
- `frontend/send`: Deploys the built files via `rsync`.
- `bs`: A shortcut to build and send in one step.

## 7. How Docker, Compose, Makefile, and Caddy Work Together

In this project, each tool has a specific role, and together they create a smooth and automated development and deployment workflow:

### 1. Dockerfile – Build the Backend Application

The `Dockerfile` in the `backend/` folder builds a statically linked Go binary and packages it into a lightweight container. This ensures that the backend runs consistently across environments—whether on your local machine or a remote server.

> ✅ **Key benefit**: Environment-independent Go app containerization.

### 2. docker-compose – Define and Run Multi-Container Setup

The `compose.yaml` file brings the whole application stack together:

* PostgreSQL for the database
* Redis for caching
* The Go backend container built from the Dockerfile

With one command (`make compose/up`), all these services are launched and networked properly.

> ✅ **Key benefit**: One file to rule them all—easy orchestration of your entire backend stack.

### 3. Makefile – Automate Everything

The `Makefile` wraps repetitive or error-prone commands—like syncing files, applying database migrations, or rebuilding the frontend—into friendly, memorable shortcuts.

Examples:

* `make bs`: Build and deploy the frontend
* `make backend/migrate/up`: Run database migrations
* `make compose/build`: Start all containers with a fresh build

> ✅ **Key benefit**: Simplifies daily dev and deployment tasks.

### 4. Caddy – Serve Everything Securely

Caddy serves both the frontend (static Vue build) and the API (reverse proxy to Go backend). It also handles HTTPS automatically with Let's Encrypt.

Caddy routes:

* `https://linze.pro` → Vue frontend
* `https://linze.pro/api/*` → Go backend
* `https://file.linze.pro` → Static file server (with CORS headers for HLS streaming)

> ✅ **Key benefit**: Zero-config HTTPS and clean domain routing.

### Full Workflow Example

1. Build containers: `make compose/build`
2. Run DB migrations: `make backend/migrate/up`
3. Build and deploy frontend: `make bs`
4. Access the app at `https://linze.pro`

## 8. Tips and Troubleshooting

Here are a few lessons learned and practical tips to save you time:

### 1. Example `.envrc` File

Make sure your `.envrc` is correctly set up for database migrations and other Makefile tasks:

```
export CLOUD_DB_DSN=postgres://<username>:<password>@postgres:5432/<dbname>
```

> **Note:** Use the Docker Compose **service name** (`postgres`) as the host, not `localhost`.

### 2. Avoid Quoting ENV Values

If you wrap environment variables in **double quotes** like this:

```
export CLOUD_DB_DSN="postgres://..."
```

…it may cause unexpected errors when passed into Makefile commands. Instead, write it without quotes unless absolutely necessary.

### 3. Use `ssh-add` Before `rsync`

If you’re deploying files with `rsync` over SSH, be sure your private key is loaded:

```bash
ssh-add ~/.ssh/id_rsa
```

> Otherwise, `rsync` may fail with a permission error, even if `ssh` works fine.

## 9. Summary

In this post, I walked you through how I combined Docker, Docker Compose, Makefile, and Caddy to build and deploy my full-stack blog project. Each tool plays a distinct role:

* **Docker** packages the Go backend into a portable container.
* **Docker Compose** coordinates backend services like Postgres and Redis.
* **Makefile** automates common dev and deployment tasks.
* **Caddy** serves both static and dynamic content with automatic HTTPS.

The result is a clean, maintainable, and production-ready setup.

🖥️ **Live Site**: [linze.pro](https://linze.pro)
📦 **Source Code**: [GitHub – nicolasleigh/linze.pro](https://github.com/nicolasleigh/linze.pro)

