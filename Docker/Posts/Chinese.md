# 如何使用 Docker、Docker Compose、Makefile 和 Caddy 部署全栈应用 —— 以该博客为例

## 1. 项目概述

linze.pro 是我的个人博客应用，它允许我发布和管理博客文章，访问者可以浏览文章、查看作品集项目以及了解更多关于我的信息。它被设计为一个全栈应用，专注于清晰的架构、多语言支持和快速的性能。 为了使开发和部署更加高效，该项目集成了以下工具：

* Dockerfile – 用于后端容器化。

* Docker Compose – 用于编排和管理多容器服务。

* Makefile – 用于定义简单的命令来构建、运行和维护应用程序。

* Caddy – 用于提供静态前端文件并将API请求反向代理到后端，支持HTTPS。 

该项目是一个全栈应用：

* 前端：使用Vue和Tailwind CSS构建，提供响应式和用户友好的界面。

* 后端：使用Go语言构建，包含处理身份验证、博客文章的增删改查操作、数据分析等功能。 

以下是项目文件夹结构：

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

以下是 Go 后端的 `Dockerfile`：

  ```dockerfile
  FROM golang:1.23.8-alpine3.20
  
  WORKDIR /usr/src/app
  
  COPY go.mod go.sum ./
  
  # 使用阿里巴巴的 Go 代理以在中国境内更快地下载模块
  RUN go env -w GOPROXY=https://mirrors.aliyun.com/goproxy/,direct
  
  RUN go mod download
  
  # 安装数据库的迁移工具
  RUN go install -tags 'postgres' github.com/golang-migrate/migrate/v4/cmd/migrate@latest
  
  # 将 `migrate` 链接到通用路径
  RUN ln -s /go/bin/linux_amd64/migrate /usr/local/bin/migrate
  
  # 复制源代码
  COPY . .
  
  # 将应用构建到 `/usr/local/bin/app`
  RUN go build -o /usr/local/bin/app ./cmd/api
  
  # 暴露端口
  EXPOSE 8085
  
  # 运行构建好的二进制文件
  CMD ["app"]
  ```

  ## 4. compose.yaml

为了编排 `linze.pro` 博客应用中的所有服务，我使用 Docker Compose。以下是 `compose.yaml` 文件：

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

### 服务说明

#### `postgres`

* 在基于 Alpine 的轻量级容器中运行 PostgreSQL。
* 使用 `secrets` 安全地存储数据库密码。
* 通过 `volumes` 存储数据库数据。
* 连接到 `backend_network`。

#### `backend`

* **从 `Dockerfile` 构建Go后端。**
* **从 `.envrc` 加载环境变量。**
* **依赖于 `postgres`（确保PostgreSQL先启动）。**
* **绑定到 `127.0.0.1:8085`，仅限于主机访问。**
* **与PostgreSQL共享相同的网络以实现容器间通信。**

#### `redis`

* **用于缓存或速率限制的轻量级内存存储。**

## 5. Caddy设置

Caddy是一个功能强大的Web服务器，它支持自动HTTPS、反向代理和静态文件服务。以下是在`linze.pro`项目中使用的`Caddyfile`：

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

### `Caddyfile`解析

#### `linze.pro`

该部分配置了用于提供前端服务并处理API路由的主站点。

* `handle /api/*`:

  对诸如 `/api/v1/posts` 或 `/api/v1/auth` 等请求被反向代理到运行在 `localhost:8085` 上的后端Go服务器。

* `handle`:

  对所有其他路由（例如，`/`、`/posts`）提供由Vue构建的静态文件

  * `root`: 指向Vue的构建文件。
  * `try_files {path} /index.html`: 指向前端代码入口点。
  * `file_server`: 启用静态文件服务。

#### `file.linze.pro`

该子域名用于提供静态资源（图片和视频）

* `root`: 指向服务器上的 `/resources` 文件夹。
* `header`: 设置 CORS 头，允许其他来源访问资源。因为我的项目使用HLS流式传输视频，这涉及通过 JavaScript 加载 `.m3u8` 和 `.ts` 文件，所以必需设置该 CORS 头。如果你的视频是普通 MP4 文件，则通常不需要此 CORS 配置。
* `file_server browse`: 启用目录列表。

## 6. Makefile

`Makefile` 用于自动化频繁任务，如Docker编排、数据库管理、前端部署和远程同步。以下是其结构： 

### 变量和环境

```makefile
include ./backend/.envrc
MIGRATIONS_PATH=./cmd/migrate/migrations
```

- `include ./backend/.envrc`: 加载环境变量，如数据库DSN。
- `MIGRATIONS_PATH`: 定义数据库迁移文件的相对路径。由于`migrate`命令在后端Docker容器内执行，`.`指的是后端文件夹的根目录。

### 资源部署

```makefile
.PHONY: res/send
res/send:
	@rsync -rP resources nicolas@106.14.126.186:~/linze.pro
```

通过 `rsync` 将 `resources` 文件夹同步到远程服务器。

- 我使用 `rsync` 而不是 `scp`，因为 `rsync` 更强大、更可靠。根据我的经验，`scp` 的行为不太一致——比如当我将 `dist` 文件夹发送到远程服务器时，第一次不会自动创建该文件夹，而第二次却会自动创建。这种不确定性让我转而使用 `rsync`，它在文件传输过程中表现得更加稳定和高效。

- `-r`: 递归复制整个目录。

- `-P`: 显示传输进度。

`.PHONY: res/send`

- 将 `res/send` 声明为一个 **伪目标**，表示它不是一个真实存在的文件或目录。如果当前目录下真的存在一个名为 `res/send` 的文件或文件夹，而没有 `.PHONY` 声明，`make` 会跳过执行命令。标记为 `.PHONY` 可以强制 `make` 每次都执行该命令。

`@rsync` 前的 `@` 符号

- `@` 符号用于抑制 `make` 默认的命令打印行为。加上 `@` 后，命令本身不会显示在终端中，只会输出 `rsync` 的实际运行结果和进度信息，使输出更加简洁。

### Docker Compose 命令说明

```makefile
.PHONY: compose/build
compose/build:
	sudo docker compose up --build

.PHONY: compose/up
compose/up:
	sudo docker compose up -d
```

- `compose/build`: 构建镜像并以交互模式运行容器。
- `compose/up`: 以后台（detached）模式运行容器。

### PostgreSQL 数据库工具命令

```makefile
.PHONY: backend/createdb
backend/createdb:
	sudo docker exec -it blog-postgres createdb -U nicolas blog

.PHONY: backend/psql
backend/psql:
	sudo docker exec -it blog-postgres psql -U nicolas blog
```

* 在 Docker 容器内运行常用的 Postgres 命令行工具，方便进行数据库管理操作。

### 数据库迁移

```makefile
.PHONY: backend/migrate/up
backend/migrate/up:
	sudo docker exec blog-backend migrate -database ${CLOUD_DB_DSN} -path ${MIGRATIONS_PATH} up
```

- 使用 [golang-migrate](https://github.com/golang-migrate/migrate) 工具执行数据库迁移操作。
- `CLOUD_DB_DSN` 变量在 backend 文件夹中的 `.envrc` 文件里定义，用于指定数据库连接信息。

### 前端构建与部署

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

- `frontend/build`: 构建 Vue 应用。
- `frontend/send`: 通过 `rsync` 将构建后的文件部署到远程服务器。
- `bs`: 构建并发送的一键快捷命令。

## 7. Docker、Compose、Makefile 与 Caddy 如何协同工作

在本项目中，每个工具各司其职，共同构建出一套流畅、自动化的开发与部署流程：

### 1. Dockerfile – 构建后端应用

位于 `backend/` 文件夹中的 `Dockerfile` 会将 Go 应用编译为静态链接的二进制文件，并打包进轻量级的容器中。这确保了无论在本地还是服务器环境中，后端运行表现都一致。

> ✅ **主要优势**：实现 Go 应用的跨环境一致运行。

### 2. docker-compose – 定义并运行多容器服务

`compose.yaml` 文件将整个应用栈组织起来：

* PostgreSQL：用于数据库存储
* Redis：用于缓存处理
* 从 Dockerfile 构建的 Go 后端容器

只需一条命令（如 `make compose/up`），即可启动并联网所有服务。

> ✅ **主要优势**：一个文件统一管理整个后端栈，部署更简单。

### 3. Makefile – 自动化一切流程

`Makefile` 将那些重复、容易出错的命令（比如同步资源、执行数据库迁移、构建前端）封装成简洁好记的指令。

例如：

* `make bs`：构建并部署前端
* `make backend/migrate/up`：执行数据库迁移
* `make compose/build`：构建并启动所有容器

> ✅ **主要优势**：大大简化日常开发和部署操作。

### 4. Caddy – 安全地服务所有内容

Caddy 负责服务静态前端（Vue 构建的文件）和后端 API（通过反向代理），并通过 Let's Encrypt 自动配置 HTTPS。

Caddy 路由规则：

* `https://linze.pro` → Vue 前端
* `https://linze.pro/api/*` → Go 后端 API
* `https://file.linze.pro` → 静态资源服务器（带有 HLS 流媒体所需的 CORS 头）

> ✅ **主要优势**：零配置 HTTPS + 干净直观的域名路由。

### 完整工作流示例

1. 构建容器：`make compose/build`
2. 执行数据库迁移：`make backend/migrate/up`
3. 构建并部署前端：`make bs`
4. 访问应用地址：`https://linze.pro`

## 8. 小贴士与问题排查

以下是我在开发过程中踩过的坑和总结出的一些实用经验，希望能帮你少走弯路：

### 1. `.envrc` 文件示例

确保你的 `.envrc` 文件配置正确，尤其是用于数据库迁移和 Makefile 命令时：

```
export CLOUD_DB_DSN=postgres://<用户名>:<密码>@postgres:5432/<数据库名>
```

> **注意**：主机名请使用 Docker Compose 中定义的服务名（比如这里是 `postgres`），**不要**使用 `localhost`。

### 2. 避免对环境变量加引号

如果你像下面这样使用 **双引号** 包裹环境变量：

```
export CLOUD_DB_DSN="postgres://..."
```

在通过 Makefile 命令传递这些变量时，可能会出现意料之外的错误。除非特别需要，否则建议**不要加引号**。

### 3. 使用 `rsync` 前先执行 `ssh-add`

如果你通过 SSH 使用 `rsync` 发送文件，确保你已经加载了 SSH 私钥：

```bash
ssh-add ~/.ssh/id_rsa
```

> 否则，即使你能通过 `ssh` 登录，`rsync` 也可能会因权限错误而失败。

## 9. 总结

在这篇文章中，我分享了我是如何将 Docker、Docker Compose、Makefile 和 Caddy 结合在一起，构建并部署我的全栈博客项目的。每个工具都有它明确的职责：

- **Docker**：将 Go 后端打包成可移植的容器。
- **Docker Compose**：协调后端服务（如 Postgres 和 Redis），实现一键启动。
- **Makefile**：封装常用的开发与部署命令，提升效率，避免出错。
- **Caddy**：同时服务静态和动态内容，并自动配置 HTTPS 证书。

🖥️ 在线网站：[linze.pro](https://linze.pro)
📦 源代码：[GitHub – nicolasleigh/linze.pro](https://github.com/nicolasleigh/linze.pro)
