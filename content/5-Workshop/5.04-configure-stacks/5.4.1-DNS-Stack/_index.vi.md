---
title: "5.4.1 DNS Stack"
weight: 1
---
---

# DNS Stack - Route 53 Hosted Zone

## Tổng quan

DNS Stack là **lớp nền tảng** (Phase 1) của hạ tầng EveryoneCook. Nó quản lý Route 53 Hosted Zone cho domain `everyonecook.cloud`, cung cấp hạ tầng DNS mà tất cả các stacks khác phụ thuộc vào.

**Thứ tự triển khai**: Stack này **PHẢI** được triển khai đầu tiên trước bất kỳ stacks nào khác.

### Trách nhiệm chính

- Tạo và quản lý Route 53 Public Hosted Zone
- Export Hosted Zone ID và name cho cross-stack references
- Cung cấp nameservers cho domain delegation từ Hostinger

### Stack này KHÔNG bao gồm

- SES Email Identity (quản lý bởi Auth Stack - Phase 3)
- DKIM/SPF/DMARC records (quản lý bởi Auth Stack - Phase 3)
- ACM Certificates (quản lý bởi Certificate Stack - Phase 1.5)
- Application DNS records (quản lý bởi các stacks tương ứng)

---

## Kiến trúc

```
┌─────────────────────────────────────────────────────────┐
│                    Hostinger Domain                      │
│                  everyonecook.cloud                      │
│                                                          │
│  ┌────────────────────────────────────────────────┐    │
│  │  Cài đặt Domain Registrar                       │    │
│  │  • Cập nhật Nameservers sang Route 53 NS       │    │
│  └────────────────┬───────────────────────────────┘    │
└───────────────────┼──────────────────────────────────────┘
                    │ DNS Delegation
                    ▼
┌─────────────────────────────────────────────────────────┐
│           AWS Route 53 Hosted Zone                       │
│           everyonecook.cloud                             │
│                                                          │
│  Tài nguyên được tạo:                                   │
│  • Public Hosted Zone                                   │
│  • 4 Nameserver (NS) Records                            │
│  • SOA Record (tự động)                                 │
│                                                          │
│  Exports:                                                │
│  • Hosted Zone ID → Dùng bởi Certificate Stack         │
│  • Hosted Zone Name → Dùng bởi các stacks khác         │
│  • Nameservers → Cấu hình tại Hostinger                │
└─────────────────────────────────────────────────────────┘
```

---

## Stack Outputs

Sau khi triển khai, stack exports các giá trị sau:

| Tên Output        | Giá trị                                         | Sử dụng                                      |
| ----------------- | ----------------------------------------------- | -------------------------------------------- |
| `HostedZoneId`   | `Z0123456789ABCDEFGHIJ`                       | Dùng bởi Certificate Stack cho DNS validation |
| `HostedZoneName` | `everyonecook.cloud`                          | Dùng bởi các stacks khác để tạo DNS records   |
| `NameServers`    | `ns-1.awsdns-01.com, ns-2.awsdns-02.org, ...` | Cấu hình tại Hostinger cho DNS delegation    |

---

## Các bước triển khai

### Bước 1: Xem lại cấu hình

Di chuyển đến thư mục infrastructure:

```powershell
cd D:\Project_AWS\everyonecook\infrastructure
```

### Bước 2: Triển khai DNS Stack

Triển khai DNS stack lên AWS:

```powershell
# Triển khai chỉ DNS Stack
npx cdk deploy EveryoneCook-dev-DNS --context environment=dev
```

### Bước 3: Xác minh trên AWS Console

1. Điều hướng đến **Route 53** trong AWS Console
2. Vào **Hosted zones**
3. Xác minh hosted zone `everyonecook.cloud` đã được tạo

**Kết quả mong đợi**:

- **Domain name**: `everyonecook.cloud`
- **Type**: Public hosted zone
- **Records**: 2 (NS và SOA records - tự động tạo)

![Route 53 Hosted Zone](/images/5-Workshop/5.4-configure-stacks/host_router53.jpg)

### Bước 4: Copy Nameservers

Từ CloudFormation Outputs hoặc Route 53 console, copy tất cả 4 nameserver records:

```
ns-123.awsdns-45.com
ns-678.awsdns-90.net
ns-1234.awsdns-56.org
ns-5678.awsdns-01.co.uk
```

![Route 53 Nameservers](/images/5-Workshop/5.2-setup-environment/DNS_record.png)

---

## Cấu hình Hostinger

### Bước quan trọng sau triển khai

⚠️ **QUAN TRỌNG**: Sau khi triển khai DNS Stack, bạn **PHẢI** cập nhật nameservers tại Hostinger để ủy quyền quản lý DNS cho Route 53.

### Cập nhật Nameservers tại Hostinger

1. **Đăng nhập Hostinger hPanel**
   - Truy cập [https://hpanel.hostinger.com](https://hpanel.hostinger.com)
   - Đăng nhập với credentials Hostinger của bạn

2. **Truy cập Domain Management**
   - Vào phần **Domains**
   - Chọn domain `everyonecook.cloud`

3. **Thay đổi Nameservers**
   - Click vào **DNS/Nameservers**
   - Chọn **Change nameservers**
   - Chọn **Custom nameservers**

4. **Nhập Route 53 Nameservers**
   - Nameserver 1: `ns-123.awsdns-45.com`
   - Nameserver 2: `ns-678.awsdns-90.net`
   - Nameserver 3: `ns-1234.awsdns-56.org`
   - Nameserver 4: `ns-5678.awsdns-01.co.uk`

5. **Lưu cấu hình**
   - Click nút **Change nameservers**
   - Chờ xác nhận

### Thời gian Propagation

- **Propagation ban đầu**: 15-30 phút
- **Propagation toàn cầu đầy đủ**: Đến 48 giờ (thường trong 2-4 giờ)

### Xác minh DNS Delegation

Sau khi cập nhật nameservers, xác minh delegation:

```powershell
# Kiểm tra nameservers cho domain
nslookup -type=NS everyonecook.cloud
```

---

## Phân tích chi phí

### Chi phí hàng tháng

| Tài nguyên                     | Chi phí                     | Ghi chú                             |
| ------------------------------ | --------------------------- | ----------------------------------- |
| **Route 53 Hosted Zone** | $0.50/tháng                 | Chi phí cố định mỗi hosted zone     |
| **DNS Queries**          | $0.40 mỗi triệu queries     | 1 tỷ queries đầu tiên/tháng         |
| **Tổng (Ước tính)**      | **~$0.50-1.00/tháng** | Traffic rất thấp trong môi trường dev |

---

## Cross-Stack Dependencies

### Exports được sử dụng bởi các Stacks khác

DNS Stack exports các giá trị được import bởi:

1. **Certificate Stack** (Phase 1.5)
   - Imports: `HostedZoneId`
   - Mục đích: Tạo DNS validation records cho ACM certificates

2. **Core Stack** (Phase 2)
   - Imports: `HostedZoneId`, `HostedZoneName`
   - Mục đích: Tạo CloudFront A/AAAA alias records

3. **Backend Stack** (Phase 4)
   - Imports: `HostedZoneId`
   - Mục đích: Tạo API Gateway custom domain DNS records

---

## Checklist Validation

Trước khi tiến hành triển khai Certificate Stack:

- [ ] DNS Stack triển khai thành công lên AWS
- [ ] Route 53 Hosted Zone hiển thị trong AWS Console
- [ ] 4 nameserver records đã lấy từ stack outputs
- [ ] Nameservers đã cập nhật tại Hostinger hPanel
- [ ] DNS delegation đã xác minh với `nslookup`
- [ ] Stack exports hiển thị trong CloudFormation console

---

## Bước tiếp theo

Sau khi triển khai và cấu hình DNS Stack thành công:

➡️ **[5.4.2 Certificate Stack](../5.4.2-Certificate-Stack/)** - Tạo ACM certificates với DNS validation

Certificate Stack sẽ:

- Tạo ACM certificate cho CloudFront (`cdn.everyonecook.cloud`)
- Tạo wildcard ACM certificate cho API Gateway (`*.everyonecook.cloud`)
- Tự động tạo DNS validation records trong Route 53
- Phải triển khai tại region **us-east-1** (yêu cầu của CloudFront)
