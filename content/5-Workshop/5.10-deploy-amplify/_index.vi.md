---
title: "Triển khai lên Amplify"
date: 2025-12-01
weight: 10
chapter: false
pre: " <b> 5.10. </b> "
---

#### Tổng quan

Trong bước này, bạn sẽ deploy frontend Next.js application lên AWS Amplify.

#### Thiết lập Amplify

1. Vào **AWS Amplify** Console
2. Click **New app** > **Host web app**
3. Chọn **GitLab** làm source
4. Authorize GitLab access
5. Chọn repository `everyonecook`
6. Chọn branch `main`

#### Cấu hình Build Settings

```yaml
version: 1
frontend:
  phases:
    preBuild:
      commands:
        - cd frontend
        - npm ci
    build:
      commands:
        - npm run build
  artifacts:
    baseDirectory: frontend/.next
    files:
      - '**/*'
  cache:
    paths:
      - frontend/node_modules/**/*
```

#### Environment Variables

Thêm các environment variables trong Amplify Console:

| Variable | Value |
|----------|-------|
| `NEXT_PUBLIC_API_URL` | `https://api-dev.everyonecook.cloud` |
| `NEXT_PUBLIC_CDN_URL` | `https://cdn-dev.everyonecook.cloud` |
| `NEXT_PUBLIC_COGNITO_USER_POOL_ID` | `ap-southeast-1_XXXXXXXX` |
| `NEXT_PUBLIC_COGNITO_CLIENT_ID` | `xxxxxxxxxxxxxxxxxxxxxxxxxx` |

#### Custom Domain

1. Vào **Domain management**
2. Click **Add domain**
3. Nhập `dev.everyonecook.cloud`
4. Cấu hình SSL certificate

#### Deploy

Amplify tự động deploy khi push code lên GitLab.

---

### Bước tiếp theo

➡️ **[5.11 - Dọn dẹp tài nguyên](../5.11-cleanup/)** - Xóa tài nguyên để tránh chi phí
