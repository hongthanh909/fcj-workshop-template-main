---
title: "5.4.2 Certificate Stack"
weight: 2
---
---
# Certificate Stack - Chứng chỉ ACM cho SSL/TLS

## Tổng quan

Certificate Stack là **lớp hạ tầng Phase 1.5** của dự án EveryoneCook. Nó quản lý các chứng chỉ AWS Certificate Manager (ACM) cho CloudFront CDN và API Gateway, cung cấp mã hóa SSL/TLS cho tất cả traffic HTTPS.

**Thứ tự triển khai**: Stack này **PHẢI** được triển khai sau DNS Stack và **trước** Core Stack và Backend Stack.

**Yêu cầu Region quan trọng**: Stack này **PHẢI** được triển khai tại region **us-east-1** vì CloudFront là dịch vụ global chỉ có thể truy cập ACM certificates từ us-east-1.

### Trách nhiệm chính

- Tạo ACM certificate cho CloudFront CDN (`cdn.everyonecook.cloud` hoặc `cdn-dev.everyonecook.cloud`)
- Tạo wildcard ACM certificate cho API Gateway (`*.everyonecook.cloud`)
- Tự động DNS validation qua Route 53
- Export certificate ARNs cho Core Stack và Backend Stack

---

## Kiến trúc

```
┌─────────────────────────────────────────────────────────────────┐
│                    Route 53 Hosted Zone                          │
│                    everyonecook.cloud                            │
│                    (từ DNS Stack)                                │
└───────────────────┬─────────────────────────────────────────────┘
                    │ DNS Validation
                    ▼
┌─────────────────────────────────────────────────────────────────┐
│            AWS Certificate Manager (us-east-1)                   │
│                                                                  │
│  Certificate 1: CloudFront Certificate                          │
│  ├─ Domain: cdn.everyonecook.cloud (hoặc cdn-dev)              │
│  ├─ Validation: DNS (Route 53)                                 │
│  └─ Export: CloudFrontCertificateArn                           │
│                                                                  │
│  Certificate 2: API Gateway Wildcard Certificate               │
│  ├─ Domain: *.everyonecook.cloud                               │
│  ├─ SAN: everyonecook.cloud                                    │
│  └─ Export: ApiGatewayCertificateArn                           │
└─────────────────────────────────────────────────────────────────┘
```

---

## Stack Outputs

Sau khi triển khai, stack exports các giá trị sau:

| Tên Output                     | Giá trị                                                                     | Sử dụng                                          |
| ------------------------------ | --------------------------------------------------------------------------- | ------------------------------------------------ |
| `CloudFrontCertificateArn`   | `arn:aws:acm:us-east-1:616580903213:certificate/8d53776e-...`            | Dùng bởi Core Stack cho CloudFront distribution   |
| `ApiGatewayCertificateArn`   | `arn:aws:acm:us-east-1:616580903213:certificate/a1b2c3d4-...`            | Dùng bởi Backend Stack cho API Gateway domain     |

---

## Các bước triển khai

### Bước 1: Xác minh điều kiện tiên quyết

Trước khi triển khai Certificate Stack, đảm bảo:

-  DNS Stack đã triển khai thành công
-  Route 53 Hosted Zone tồn tại với nameservers đúng
-  DNS delegation từ Hostinger đã hoàn thành và propagated

Xác minh DNS đang hoạt động:

```powershell
nslookup -type=NS everyonecook.cloud
```

### Bước 2: Triển khai Certificate Stack

Di chuyển đến thư mục infrastructure:

```powershell
cd D:\Project_AWS\everyonecook\infrastructure
```

Triển khai Certificate Stack tại **us-east-1**:

```powershell
# Triển khai Certificate Stack
npx cdk deploy EveryoneCook-dev-Certificate --context environment=dev
```

**Quan trọng**: Lưu ý region là **us-east-1**, không phải ap-southeast-1.

### Bước 3: Chờ Certificate Validation

ACM certificates yêu cầu DNS validation. Quá trình này mất 5-10 phút:

1. ACM tạo CNAME validation records trong Route 53
2. Route 53 phản hồi với validation data
3. ACM xác minh và cấp certificates
4. Status chuyển từ "Pending validation" sang "Issued"

### Bước 4: Xác minh trên AWS Console

#### Điều hướng đến ACM (region us-east-1)

1. Mở AWS Console
2. **QUAN TRỌNG**: Chuyển region sang **N. Virginia (us-east-1)**
3. Điều hướng đến **Certificate Manager**

![Chuyển sang region us-east-1](/images/5-Workshop/5.4-configure-stacks/switch-region-us-east-1.png)

#### Xác minh CloudFront Certificate

1. Tìm certificate với domain `cdn-dev.everyonecook.cloud`
2. Kiểm tra status là **Issued** 
3. Xác minh DNS validation records có mặt

![CloudFront ACM Certificate](/images/5-Workshop/5.4-configure-stacks/acm-cloudfront-cert.png)

#### Xác minh API Gateway Wildcard Certificate

1. Tìm certificate với domain `*.everyonecook.cloud`
2. Kiểm tra status là **Issued** 
3. Xác minh nó bao gồm wildcard và SAN (everyonecook.cloud)

![API Gateway Wildcard Certificate](/images/5-Workshop/5.4-configure-stacks/acm-api-wildcard-cert.png)

#### Xác minh DNS Validation Records trong Route 53

1. Điều hướng đến **Route 53** > **Hosted zones**
2. Chọn hosted zone `everyonecook.cloud`
3. Xác minh CNAME validation records đã được tạo

![Route 53 ACM Validation Records](/images/5-Workshop/5.4-configure-stacks/route53-acm-validation.png)

---

## Phân tích chi phí

### Chi phí hàng tháng

| Tài nguyên                         | Chi phí           | Ghi chú                                    |
| ---------------------------------- | ----------------- | ------------------------------------------ |
| **ACM Certificates**         | **$0/tháng**    | Miễn phí cho certificates dùng với AWS services |
| **DNS Validation Records**   | **$0/tháng**    | Bao gồm trong chi phí Route 53 hosted zone |
| **Certificate Renewal**      | **$0/tháng**    | Tự động gia hạn (miễn phí)                 |
| **Tổng**                     | **$0/tháng**    | 100% miễn phí                              |

---

## Cross-Stack Dependencies

### Exports được sử dụng bởi các Stacks khác

1. **Core Stack** (Phase 2)
   - Imports: `CloudFrontCertificateArn`
   - Mục đích: Gắn SSL certificate vào CloudFront distribution

2. **Backend Stack** (Phase 4)
   - Imports: `ApiGatewayCertificateArn`
   - Mục đích: Tạo API Gateway custom domain với SSL

---

## Checklist Validation

Trước khi tiến hành triển khai Core Stack:

- [ ] Certificate Stack đã triển khai tại region **us-east-1**
- [ ] CloudFront certificate status là **Issued** 
- [ ] API Gateway wildcard certificate status là **Issued** 
- [ ] DNS validation CNAME records hiển thị trong Route 53
- [ ] Certificate ARNs đã exported trong CloudFormation outputs

---

## Bước tiếp theo

Sau khi triển khai Certificate Stack thành công:

➡️ **[5.4.3 Core Stack](../5.4.3-Core-Stack/)** - Tạo hạ tầng DynamoDB, S3, và CloudFront

Core Stack sẽ:

- Tạo DynamoDB Single Table với encryption
- Tạo S3 buckets cho content và CDN logs
- Tạo CloudFront distribution với SSL certificate
- Import `CloudFrontCertificateArn` từ stack này
- Cấu hình custom domain `cdn.everyonecook.cloud`
