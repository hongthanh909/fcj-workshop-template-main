---
title: "5.4.3 Core Stack"
weight: 3
---
---
# Core Stack - Hạ tầng Data Layer

## Tổng quan

Core Stack là **lớp dữ liệu nền tảng Phase 2** của dự án EveryoneCook. Nó tạo hạ tầng core cho lưu trữ dữ liệu, phân phối nội dung, và mã hóa mà tất cả các dịch vụ khác phụ thuộc vào.

**Thứ tự triển khai**: Stack này **PHẢI** được triển khai sau Certificate Stack và **trước** Auth Stack và Backend Stack.

### Trách nhiệm chính

- Tạo DynamoDB Single Table với 5 GSI indexes cho username-based queries
- Tạo S3 buckets với Intelligent-Tiering để tối ưu chi phí (tiết kiệm 57%)
- Tạo CloudFront CDN distribution với compression và caching
- Tạo KMS encryption keys cho DynamoDB và S3
- Cấu hình Origin Access Control (OAC) cho bảo mật S3
- Thiết lập custom domain với SSL certificate cho CDN

### Stack này bao gồm

**Storage**:
- DynamoDB Single Table với encryption at rest
- S3 Content Bucket (avatars, posts, recipes, backgrounds)
- S3 CDN Logs Bucket cho CloudFront access logs

**CDN & Caching**:
- CloudFront Distribution với custom domain
- Compression enabled (Gzip + Brotli, giảm 70-80% kích thước)
- Cache policies (24h default, 7 days max)
- Origin Access Control (OAC) cho bảo mật S3

**Security**:
- KMS Customer Managed Keys cho encryption
- Automatic key rotation (hàng năm)
- Security headers (HSTS, X-Frame-Options, v.v.)

**Tối ưu chi phí**:
- S3 Intelligent-Tiering (giảm 57% chi phí)
- CloudFront Price Class 200 (giảm 45% chi phí)
- Pay-per-request billing cho môi trường dev

---

## Các bước triển khai

### Bước 1: Xác minh điều kiện tiên quyết

Trước khi triển khai Core Stack, đảm bảo:

-  DNS Stack đã triển khai thành công
-  Certificate Stack đã triển khai thành công (us-east-1)
-  CloudFront certificate status là **Issued**
-  Route 53 DNS delegation đang hoạt động

### Bước 2: Triển khai Core Stack

Di chuyển đến thư mục infrastructure:

```powershell
cd D:\Project_AWS\everyonecook\infrastructure
```

Triển khai Core Stack tại **ap-southeast-1**:

```powershell
# Triển khai Core Stack
npx cdk deploy EveryoneCook-dev-Core --context environment=dev
```

### Bước 3: Chờ CloudFront Distribution

CloudFront distribution deployment mất **15-20 phút**. Bạn có thể theo dõi tiến trình:

```powershell
# Kiểm tra CloudFront distribution status
aws cloudfront get-distribution --id E2ABCDEFGHIJKL --query 'Distribution.Status'
```

### Bước 4: Xác minh trên AWS Console

#### Điều hướng đến DynamoDB

1. Mở AWS Console → region **ap-southeast-1**
2. Vào **DynamoDB** > **Tables**
3. Tìm table `EveryoneCook-dev-v2`

![DynamoDB Single Table](/images/5-Workshop/5.4-configure-stacks/dynamodb-table.png)

**Xác minh**:
-  Partition key: PK (String)
-  Sort key: SK (String)
-  5 GSI indexes hiển thị
-  Billing mode: PAY_PER_REQUEST (dev)
-  Encryption: Customer managed KMS
-  Streams: Enabled (NEW_AND_OLD_IMAGES)

#### Điều hướng đến S3

1. Vào **S3** > **Buckets**
2. Tìm buckets:
   - `everyonecook-content-dev`
   - `everyonecook-cdn-logs-dev`

![S3 Content Bucket](/images/5-Workshop/5.4-configure-stacks/s3-content-bucket.png)

**Xác minh Content Bucket**:
-  Block all public access: On
-  Versioning: Enabled (prod) / Disabled (dev)
-  Intelligent-Tiering: Configured
-  Encryption: S3-managed keys
-  CORS: Configured
-  Lifecycle rules: 2 rules

![S3 Intelligent-Tiering](/images/5-Workshop/5.4-configure-stacks/s3-intelligent-tiering.png)

#### Điều hướng đến CloudFront

1. Vào **CloudFront** > **Distributions**
2. Tìm distribution với domain `cdn-dev.everyonecook.cloud`

![CloudFront Distribution](/images/5-Workshop/5.4-configure-stacks/cloudfront-distribution.png)

**Xác minh**:
-  Status: Deployed
-  Domain: cdn-dev.everyonecook.cloud
-  Origin: S3 bucket với OAC
-  Compression: Enabled
-  Price Class: 200
-  SSL Certificate: Valid
-  HTTPS: Redirect

![CloudFront OAC](/images/5-Workshop/5.4-configure-stacks/cloudfront-oac.png)

#### Điều hướng đến KMS

1. Vào **KMS** > **Customer managed keys**
2. Tìm keys:
   - `everyonecook-dynamodb-dev`
   - `everyonecook-s3-dev`

![KMS Customer Managed Keys](/images/5-Workshop/5.4-configure-stacks/kms-keys.png)

**Xác minh**:
-  Key rotation: Enabled
-  Key state: Enabled
-  Deletion protection: Configured
-  Key policy: DynamoDB/S3 service access

#### Xác minh Route 53 DNS Record

1. Vào **Route 53** > **Hosted zones**
2. Chọn `everyonecook.cloud`
3. Xác minh A record cho `cdn-dev.everyonecook.cloud`

![Route 53 CloudFront Alias](/images/5-Workshop/5.4-configure-stacks/route53-cloudfront-alias.png)

---

## Phân tích chi phí

### Chi phí hàng tháng (Môi trường Dev)

| Tài nguyên | Cấu hình | Chi phí hàng tháng | Ghi chú |
|----------|---------------|--------------|-------|
| **DynamoDB** | PAY_PER_REQUEST | $1-5 | Traffic thấp |
| **S3 Content** | 10GB, Intelligent-Tiering | $0.23 | Chuyển sang Deep Archive |
| **S3 Logs** | 5GB | $0.12 | Access logs |
| **CloudFront** | 100GB transfer, Price Class 200 | $4.70 | Tiết kiệm 45% so với All |
| **KMS** | 2 keys | $2.00 | $1/key/tháng |
| **Route 53** | 1 hosted zone, DNS queries | $0.50 | Từ DNS Stack |
| **Tổng (Ước tính)** | | **~$8-13/tháng** | Kịch bản traffic thấp |

---

## Checklist Validation

Trước khi tiến hành triển khai Auth Stack:

- [ ] Core Stack đã triển khai thành công tại ap-southeast-1
- [ ] DynamoDB table tồn tại với 5 GSI indexes
- [ ] S3 content bucket có Intelligent-Tiering configured
- [ ] S3 CORS configured cho frontend domain
- [ ] CloudFront distribution status là **Deployed**
- [ ] CloudFront custom domain resolves: `cdn-dev.everyonecook.cloud`
- [ ] CloudFront OAC configured cho S3 access
- [ ] KMS keys đã tạo với rotation enabled
- [ ] Route 53 A record đã tạo cho CloudFront

---

## Bước tiếp theo

Sau khi triển khai Core Stack thành công:

➡️ **[5.4.4 Auth Stack](../5.4.4-Auth-Stack/)** - Tạo Cognito User Pool và authentication

Auth Stack sẽ:

- Tạo Cognito User Pool với advanced security
- Cấu hình user attributes và password policy
- Thiết lập Cognito triggers (Lambda functions)
- Tạo SES email identity cho verification emails
- Import DynamoDB table từ Core Stack
