---
title: 'Docker 容器化部署实践'
publishDate: '2024-09-05'
description: '介绍 Docker 容器化的基本概念和实践，包括 Dockerfile 编写、镜像构建、容器编排等，帮助开发者更好地使用容器技术。'
tags:
  - Docker
  - 容器化
  - DevOps
language: 'zh-CN'
draft: false
---

Docker 已经成为现代应用部署的标准方式。它通过容器化技术，实现了"一次构建，到处运行"的理念。

## Docker 基础概念

### 镜像（Image）

镜像是只读的模板，用于创建容器。可以理解为应用程序的"快照"。

### 容器（Container）

容器是镜像的运行实例，是一个轻量级的、可执行的软件包。

### Dockerfile

Dockerfile 是用于构建镜像的文本文件，包含了一系列指令。

## 编写 Dockerfile

```dockerfile
# 使用官方 Node.js 运行时作为基础镜像
FROM node:18-alpine

# 设置工作目录
WORKDIR /app

# 复制 package.json 和 package-lock.json
COPY package*.json ./

# 安装依赖
RUN npm ci --only=production

# 复制应用代码
COPY . .

# 暴露端口
EXPOSE 3000

# 定义环境变量
ENV NODE_ENV=production

# 启动应用
CMD ["node", "server.js"]
```

## 多阶段构建

多阶段构建可以减小最终镜像大小。

```dockerfile
# 构建阶段
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# 运行阶段
FROM node:18-alpine
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules
COPY package*.json ./
EXPOSE 3000
CMD ["node", "dist/server.js"]
```

## Docker Compose

Docker Compose 用于定义和运行多容器应用。

```yaml
version: '3.8'

services:
  web:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
    depends_on:
      - db
  
  db:
    image: postgres:15
    environment:
      - POSTGRES_DB=myapp
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=password
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

## 最佳实践

1. **使用 .dockerignore**：排除不需要的文件
2. **最小化层数**：合并 RUN 命令
3. **使用特定版本标签**：避免使用 `latest`
4. **非 root 用户运行**：提高安全性
5. **健康检查**：添加 HEALTHCHECK 指令

## 总结

Docker 容器化带来了部署的一致性和可移植性。通过合理的 Dockerfile 编写和容器编排，我们可以简化部署流程，提高开发效率。