---
title: "5.4.6 Frontend Stack"
weight: 6
---
---
# Frontend Stack - AWS Amplify Hosting

## Tổng quan

Frontend Stack quản lý AWS Amplify hosting cho ứng dụng Next.js 15 của EveryoneCook. Stack này là **tùy chọn** và có thể được triển khai riêng biệt.

### Trách nhiệm chính

- Hosting ứng dụng Next.js 15 trên AWS Amplify
- Cấu hình custom domain
- CI/CD pipeline từ GitLab
- Environment variables management

### Tài nguyên chính

- **Amplify App**: Next.js 15 hosting
- **Amplify Branch**: Production và preview branches
- **Custom Domain**: `dev.everyonecook.cloud`

---

## Các bước triển khai

### Bước 1: Cấu hình Amplify

1. Vào **AWS Amplify** Console
2. Tạo new app từ GitLab repository
3. Cấu hình build settings cho Next.js

### Bước 2: Cấu hình Environment Variables

Thêm các environment variables:
- `NEXT_PUBLIC_API_URL`: API Gateway URL
- `NEXT_PUBLIC_CDN_URL`: CloudFront CDN URL
- `NEXT_PUBLIC_COGNITO_USER_POOL_ID`: Cognito User Pool ID
- `NEXT_PUBLIC_COGNITO_CLIENT_ID`: Cognito Client ID

### Bước 3: Deploy

Amplify tự động deploy khi push code lên GitLab.

---

## Phân tích chi phí

| Tài nguyên | Chi phí | Ghi chú |
|----------|---------|-------|
| **Amplify Hosting** | $0-5/tháng | Free tier available |
| **Build minutes** | $0-2/tháng | 1000 minutes free |
| **Tổng** | **~$0-7/tháng** | |

---

## Bước tiếp theo

➡️ **[5.4.7 Observability Stack](../5.4.7-Observability-Stack/)** - Tạo CloudWatch dashboards và alarms
