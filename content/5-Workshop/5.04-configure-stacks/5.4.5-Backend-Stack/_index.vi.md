---
title: "5.4.5 Backend Stack"
weight: 5
---
---
# Backend Stack - API Gateway và Lambda Functions

## Tổng quan

Backend Stack là **lớp ứng dụng Phase 4** của dự án EveryoneCook. Nó quản lý API Gateway REST API, Lambda functions, SQS queues, và WAF protection cho business logic của ứng dụng.

**Thứ tự triển khai**: Stack này **PHẢI** được triển khai sau Auth Stack.

### Trách nhiệm chính

- Tạo API Gateway REST API với custom domain
- Triển khai 6 Lambda function modules
- Cấu hình SQS queues cho async processing
- Thiết lập WAF Web ACL cho API protection
- Tạo Lambda Layer cho shared dependencies

### Tài nguyên chính

- **API Gateway REST API**: Custom domain `api-dev.everyonecook.cloud`
- **Lambda Functions** (6 modules):
  - API Router: Route requests
  - Auth User: Login, register, verify
  - Social: Posts, comments, likes
  - Recipe AI: CRUD recipes, AI generation
  - Admin: User management
  - Upload: File upload to S3
- **Lambda Layer**: Shared dependencies (giảm 90% deployment size)
- **SQS Queues** (4 queues + 4 DLQs):
  - AI Queue: AI recipe generation
  - Image Processing Queue
  - Analytics Queue
  - Notification Queue
- **WAF Web ACL**: Rate limiting, AWS Managed Rules
- **Worker Lambda**: Process AI Queue messages

---

## Các bước triển khai

### Bước 1: Xác minh điều kiện tiên quyết

-  Auth Stack đã triển khai thành công
-  Cognito User Pool tồn tại

### Bước 2: Triển khai Backend Stack

```powershell
cd D:\Project_AWS\everyonecook\infrastructure

# Triển khai Backend Stack
npx cdk deploy EveryoneCook-dev-Backend --context environment=dev
```

### Bước 3: Xác minh trên AWS Console

#### Xác minh API Gateway

1. Vào **API Gateway** > **APIs**
2. Tìm API `EveryoneCook-dev`
3. Xác minh custom domain và stages

#### Xác minh Lambda Functions

1. Vào **Lambda** > **Functions**
2. Tìm các functions với prefix `EveryoneCook-dev`
3. Xác minh 6 functions + 1 worker

#### Xác minh SQS Queues

1. Vào **SQS** > **Queues**
2. Tìm 4 queues + 4 DLQs

#### Xác minh WAF

1. Vào **WAF & Shield** > **Web ACLs**
2. Tìm Web ACL cho API Gateway

---

## Phân tích chi phí

| Tài nguyên | Chi phí | Ghi chú |
|----------|---------|-------|
| **API Gateway** | $0-3/tháng | Free tier: 1M requests |
| **Lambda** | $2-8/tháng | 7 functions |
| **SQS** | $0-1/tháng | Free tier: 1M requests |
| **WAF** | $5-8/tháng | Web ACL + rules |
| **Tổng** | **~$10-25/tháng** | |

---

## Bước tiếp theo

➡️ **[5.4.7 Observability Stack](../5.4.7-Observability-Stack/)** - Tạo CloudWatch dashboards và alarms
