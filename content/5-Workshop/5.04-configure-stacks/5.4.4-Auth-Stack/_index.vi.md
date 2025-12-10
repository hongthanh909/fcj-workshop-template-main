---
title: "5.4.4 Auth Stack"
weight: 4
---
---
# Auth Stack - Xác thực và Quản lý người dùng

## Tổng quan

Auth Stack là **lớp xác thực Phase 3** của dự án EveryoneCook. Nó quản lý Amazon Cognito User Pool cho xác thực người dùng, Lambda triggers cho custom flows, và tích hợp SES cho email verification.

**Thứ tự triển khai**: Stack này **PHẢI** được triển khai sau Core Stack và **trước** Backend Stack.

### Trách nhiệm chính

- Tạo Cognito User Pool với password policy và user attributes
- Cấu hình Lambda triggers cho custom authentication flows
- Thiết lập email verification qua SES
- Export User Pool ID và Client ID cho Backend Stack

### Tài nguyên chính

- **Cognito User Pool**: Với password policy
- **Cognito User Pool Client**: OAuth flows configuration
- **Lambda Triggers** (5 functions):
  - Pre-signup: Validate username uniqueness
  - Post-confirmation: Create user profile in DynamoDB
  - Pre-authentication: Check account status
  - Post-authentication: Update last login
  - Custom message: Customize verification emails
- **CloudWatch Log Groups**: Cho mỗi Lambda trigger
- **IAM Roles**: Lambda execution roles

---

## Các bước triển khai

### Bước 1: Xác minh điều kiện tiên quyết

Trước khi triển khai Auth Stack, đảm bảo:

-  Core Stack đã triển khai thành công
-  DynamoDB table tồn tại
-  KMS keys đã tạo

### Bước 2: Triển khai Auth Stack

```powershell
cd D:\Project_AWS\everyonecook\infrastructure

# Triển khai Auth Stack
npx cdk deploy EveryoneCook-dev-Auth --context environment=dev
```

### Bước 3: Xác minh trên AWS Console

#### Xác minh Cognito User Pool

1. Vào **Cognito** > **User pools**
2. Tìm user pool `EveryoneCook-dev`
3. Xác minh cấu hình

**Xác minh**:
-  Sign-in options: Username OR Email
-  Password policy: Min 8 chars (dev)
-  MFA: Off
-  Email verification: Required
-  Lambda triggers: 5 triggers configured

---

## Phân tích chi phí

| Tài nguyên | Chi phí | Ghi chú |
|----------|---------|-------|
| **Cognito** | $0-2/tháng | Free tier: 50,000 MAU |
| **Lambda Triggers** | $0-1/tháng | Invocations minimal |
| **CloudWatch Logs** | $0.50/tháng | 7-day retention |
| **Tổng** | **~$0-3/tháng** | |

---

## Bước tiếp theo

➡️ **[5.4.5 Backend Stack](../5.4.5-Backend-Stack/)** - Tạo API Gateway và Lambda functions
