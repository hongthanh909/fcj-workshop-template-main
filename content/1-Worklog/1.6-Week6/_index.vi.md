---
title: "Worklog Tuần 6"
date: 2025-10-16
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---
## Mục tiêu Tuần 6

* Thành thạo IAM, AWS Organizations, và Service Control Policies (SCPs).
* Học encryption với KMS, ACM, và Secrets Manager.
* Hiểu S3 security, VPC networking, CloudFront, và chiến lược DR.

## Các công việc trong tuần

| Ngày | Công việc                                         | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| ---- | ------------------------------------------------- | ------------ | --------------- | ------------------ |
| 2    | IAM, Account Security & AWS Organizations         | 10/16/2025   | 10/16/2025      | tiếp tục           |
| 3    | Encryption, KMS, ACM & Secrets Manager            | 10/17/2025   | 10/17/2025      | tiếp tục           |
| 4    | S3 Storage, Security, Lifecycle & Replication     | 10/18/2025   | 10/18/2025      | tiếp tục           |
| 5    | VPC & Networking Security (SG, NACL, VPN, DX)     | 10/19/2025   | 10/19/2025      | tiếp tục           |
| 6    | CloudFront/GA, DR & Backup/Migration              | 10/20/2025   | 10/20/2025      | tiếp tục           |

## Kết quả đạt được Tuần 6

### IAM – Identity and Access Management

- Root account: chỉ setup, không bao giờ chia sẻ
- IAM Users: đại diện cho người/dịch vụ; IAM Groups tổ chức users
- Policies (JSON): định nghĩa permissions; tuân theo **Least Privilege**
- IAM Roles: cấp permissions cho AWS services (EC2, Lambda, v.v.)

### AWS Organizations

- Quản lý nhiều tài khoản AWS tập trung
- Consolidated billing: chia sẻ tiết kiệm chi phí (RI, Savings Plans)
- **Service Control Policies (SCP)**: giới hạn permissions tối đa

### Encryption, KMS, ACM & Secrets Manager

- **AWS KMS**: Dịch vụ trung tâm quản lý encryption keys
- **ACM**: Cấp và quản lý TLS/SSL certificates
- **Secrets Manager**: Lưu trữ an toàn secrets (passwords, API keys)

### S3 Storage, Security, Lifecycle & Replication

- Storage Classes: Standard, Standard-IA, Glacier, Intelligent-Tiering
- Lifecycle Rules: chuyển objects sang storage rẻ hơn
- Replication: CRR = cross-region; SRR = same-region

### VPC & Networking Security

- **Security Groups**: instance-level, stateful
- **NACLs**: subnet-level, stateless
- VPC Peering, Endpoints, Site-to-Site VPN, Direct Connect

### CloudFront, Global Accelerator, DR & Migration

- **CloudFront (CDN)**: Cache content tại edge locations
- **Global Accelerator**: Tăng tốc TCP/UDP apps
- **DR strategies**: Backup & Restore, Pilot Light, Warm Standby, Multi-Region
