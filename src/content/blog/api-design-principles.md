---
title: 'API 设计原则与实践'
publishDate: '2024-12-02'
description: '分享 RESTful API 设计的最佳实践，包括 URL 设计、HTTP 方法使用、状态码选择、版本控制等，帮助设计易用的 API。'
tags:
  - API
  - 后端
  - 设计
language: 'zh-CN'
draft: false
---

良好的 API 设计是构建优秀应用的基础。遵循一致的设计原则，可以让 API 更易理解、更易使用、更易维护。

## RESTful 设计原则

### 资源命名

使用名词表示资源，使用复数形式。

```http
# ✅ 好的设计
GET /api/users
GET /api/users/123
POST /api/users
PUT /api/users/123
DELETE /api/users/123

# ❌ 避免
GET /api/getUsers
POST /api/createUser
```

### HTTP 方法

正确使用 HTTP 方法表达操作意图。

- `GET`: 获取资源
- `POST`: 创建资源
- `PUT`: 更新资源（完整替换）
- `PATCH`: 部分更新资源
- `DELETE`: 删除资源

### 状态码

使用合适的 HTTP 状态码。

```javascript
// 成功
200 OK          // 请求成功
201 Created     // 资源创建成功
204 No Content  // 删除成功

// 客户端错误
400 Bad Request      // 请求参数错误
401 Unauthorized     // 未认证
403 Forbidden        // 无权限
404 Not Found        // 资源不存在
409 Conflict         // 资源冲突

// 服务器错误
500 Internal Server Error // 服务器错误
503 Service Unavailable   // 服务不可用
```

## 请求和响应格式

### 请求格式

```javascript
// POST /api/users
{
  "name": "John Doe",
  "email": "john@example.com",
  "age": 30
}
```

### 响应格式

保持响应格式一致。

```javascript
// 成功响应
{
  "data": {
    "id": 123,
    "name": "John Doe",
    "email": "john@example.com"
  }
}

// 错误响应
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Email is required",
    "details": {
      "field": "email"
    }
  }
}
```

## 分页和过滤

```http
# 分页
GET /api/users?page=1&limit=20

# 过滤
GET /api/users?status=active&role=admin

# 排序
GET /api/users?sort=created_at&order=desc

# 搜索
GET /api/users?q=john
```

响应格式：

```json
{
  "data": [...],
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 100,
    "totalPages": 5
  }
}
```

## 版本控制

API 版本控制有多种方式。

### URL 版本控制

```http
GET /api/v1/users
GET /api/v2/users
```

### Header 版本控制

```http
GET /api/users
Accept: application/vnd.api+json;version=1
```

## 认证和授权

```http
# 使用 Bearer Token
Authorization: Bearer <token>

# 使用 API Key
X-API-Key: <api-key>
```

## 限流和缓存

```http
# 限流响应头
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 999
X-RateLimit-Reset: 1609459200

# 缓存控制
Cache-Control: public, max-age=3600
ETag: "33a64df551425fcc55e4d42a148795d9f25f89d4"
```

## 文档

提供清晰的 API 文档。

- 使用 OpenAPI/Swagger
- 提供示例请求和响应
- 说明错误情况
- 版本变更记录

## 总结

良好的 API 设计需要遵循 RESTful 原则，保持一致性，提供清晰的文档。通过合理的资源命名、HTTP 方法使用、状态码选择等，我们可以设计出易用、易维护的 API。