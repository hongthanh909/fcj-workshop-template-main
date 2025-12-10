---
title: "Triển khai Infrastructure"
date: 2025-12-01
weight: 5
chapter: false
pre: " <b> 5.05. </b> "
---

#### Tổng quan

Trong bước này, bạn sẽ triển khai tất cả infrastructure stacks lên AWS theo đúng thứ tự dependency. Dự án EveryoneCook sử dụng **Kiến trúc 5-Stack** với AWS CDK.

#### Kiến trúc 5-Stack

```
Phase 1:   DNS Stack           → Route 53 Hosted Zone
Phase 1.5: Certificate Stack   → ACM Certificates (us-east-1)
Phase 2:   Core Stack          → DynamoDB, S3, CloudFront, KMS
Phase 3:   Auth Stack          → Cognito, SES, Lambda Triggers
Phase 4:   Backend Stack       → API Gateway, Lambda, SQS, WAF
Phase 5:   Observability Stack → CloudWatch Dashboards & Alarms
```

#### Thời gian triển khai

| Stack | Thời gian | Tài nguyên quan trọng |
|-------|-----------|----------------------|
| DNS Stack | 2-3 phút | Route 53 Hosted Zone |
| Certificate Stack | 5-10 phút | ACM Certificate (DNS validation) |
| Core Stack | 10-15 phút | CloudFront Distribution, DynamoDB |
| Auth Stack | 5-7 phút | Cognito User Pool, SES |
| Backend Stack | 8-12 phút | API Gateway, 7 Lambda Functions |
| Observability Stack | 3-5 phút | CloudWatch Dashboards |

**Tổng thời gian triển khai: 35-50 phút**

---

### Điều kiện tiên quyết

```powershell
# Kiểm tra AWS credentials
aws sts get-caller-identity

# Kiểm tra Node.js version (phải là 20.x+)
node -v

# Kiểm tra CDK version
cdk --version

# Di chuyển đến thư mục infrastructure
cd D:\Project_AWS\everyonecook\infrastructure
```

---

### Phase 1: Triển khai DNS Stack

```powershell
# Triển khai DNS Stack
npx cdk deploy EveryoneCook-dev-DNS --context environment=dev
```

**Sau khi triển khai:**
1. Lưu 4 nameservers từ output
2. Cập nhật nameservers tại domain registrar (Hostinger)
3. Chờ DNS propagation (5-15 phút)

---

### Phase 1.5: Triển khai Certificate Stack

```powershell
# Triển khai Certificate Stack (us-east-1)
npx cdk deploy EveryoneCook-dev-Certificate --context environment=dev
```

**Chờ DNS validation (5-10 phút)** cho đến khi certificates có status "Issued".

---

### Phase 2: Triển khai Core Stack

```powershell
# Triển khai Core Stack (10-15 phút)
npx cdk deploy EveryoneCook-dev-Core --context environment=dev
```

**Stack tạo:**
- DynamoDB Table với 5 GSI indexes
- 4 S3 Buckets
- CloudFront Distribution
- 2 KMS Keys

---

### Phase 3: Triển khai Auth Stack

```powershell
# Triển khai Auth Stack
npx cdk deploy EveryoneCook-dev-Auth --context environment=dev
```

**Stack tạo:**
- Cognito User Pool
- 5 Lambda Triggers
- SES Email Identity

---

### Phase 4: Triển khai Backend Stack

```powershell
# Build Lambda code trước
cd D:\Project_AWS\everyonecook
.\deploy\deploy-backend.ps1 -Environment dev

# Triển khai Backend Stack
cd infrastructure
npx cdk deploy EveryoneCook-dev-Backend --context environment=dev
```

**Stack tạo:**
- API Gateway REST API
- 7 Lambda Functions
- Lambda Layer
- 4 SQS Queues + 4 DLQs
- WAF Web ACL

---

### Phase 5: Triển khai Observability Stack

```powershell
# Triển khai Observability Stack
npx cdk deploy EveryoneCook-dev-Observability --context environment=dev
```

**Stack tạo:**
- 4 CloudWatch Dashboards
- 10+ CloudWatch Alarms
- SNS Topic

---

### Xác minh triển khai hoàn tất

```powershell
# Liệt kê tất cả stacks đã triển khai
aws cloudformation list-stacks `
  --stack-status-filter CREATE_COMPLETE UPDATE_COMPLETE `
  --query 'StackSummaries[?contains(StackName, `EveryoneCook-dev`)].StackName' `
  --output table
```

**Kết quả mong đợi: 6 stacks**
- EveryoneCook-dev-DNS
- EveryoneCook-dev-Certificate
- EveryoneCook-dev-Core
- EveryoneCook-dev-Auth
- EveryoneCook-dev-Backend
- EveryoneCook-dev-Observability

---

### Ước tính chi phí

| Dịch vụ | Chi phí/tháng |
|---------|---------------|
| DynamoDB | $2-5 |
| S3 | $1-2 |
| CloudFront | $1-2 |
| Lambda | $3-5 |
| API Gateway | $0.35 |
| Cognito | $0-5 |
| CloudWatch | $2-3 |
| WAF | $5 |
| Route 53 | $0.50 |
| KMS | $2 |
| **Tổng** | **$20-35/tháng** |

---

### Bước tiếp theo

Infrastructure deployment hoàn tất! Tiếp theo:

➡️ **[5.06 - Cấu hình API & Lambda](../5.06-configure-api-lambda/)** - Thiết lập API routes và Lambda integration
