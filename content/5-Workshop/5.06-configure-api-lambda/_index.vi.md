---
title: "Cấu hình API & Lambda"
date: 2025-12-01
weight: 6
chapter: false
pre: " <b> 5.06. </b> "
---

#### Tổng quan

Trong bước này, bạn sẽ cấu hình API Gateway routes và Lambda function integration cho ứng dụng EveryoneCook.

#### API Gateway Configuration

API Gateway REST API đã được tạo trong Backend Stack với:
- Custom domain: `api-dev.everyonecook.cloud`
- Cognito Authorizer cho protected routes
- WAF Web ACL protection

#### Lambda Functions

6 Lambda modules đã được triển khai:

| Module | Chức năng | Memory | Timeout |
|--------|-----------|--------|---------|
| API Router | Route requests | 256 MB | 30s |
| Auth User | Login, register, verify | 512 MB | 30s |
| Social | Posts, comments, likes | 512 MB | 30s |
| Recipe AI | CRUD recipes, AI generation | 1024 MB | 60s |
| Admin | User management | 512 MB | 30s |
| Upload | File upload to S3 | 256 MB | 15s |

#### API Routes

```
POST   /auth/register     → Auth User Lambda
POST   /auth/login        → Auth User Lambda
POST   /auth/verify       → Auth User Lambda
GET    /users/{username}  → Auth User Lambda
PUT    /users/{username}  → Auth User Lambda

GET    /posts             → Social Lambda
POST   /posts             → Social Lambda
GET    /posts/{id}        → Social Lambda
PUT    /posts/{id}        → Social Lambda
DELETE /posts/{id}        → Social Lambda

GET    /recipes           → Recipe AI Lambda
POST   /recipes           → Recipe AI Lambda
POST   /recipes/generate  → Recipe AI Lambda (AI)

POST   /upload/presigned  → Upload Lambda
```

---

### Bước tiếp theo

➡️ **[5.07 - Triển khai Backend](../5.07-deploy-backend/)** - Build và deploy Lambda code
