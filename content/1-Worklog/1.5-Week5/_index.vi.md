---
title: "Worklog Tuần 5"
date: 2025-10-09
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---
### **Mục tiêu Tuần 5**

* Hiểu IAM cơ bản: users, roles, và permission policies.
* Học cách Amazon Cognito xử lý xác thực người dùng ứng dụng.
* Làm quen với AWS Organizations cho quản lý multi-account.
* Sử dụng AWS Identity Center (SSO) cho kiểm soát truy cập tập trung.
* Hiểu KMS cho quản lý encryption key.
* Phân biệt các mức trách nhiệm qua các dịch vụ AWS.
* Nắm vững Shared Responsibility Model và xu hướng chuyển sang managed services.

### Các công việc trong tuần:

| Ngày | Công việc                | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo                                                                                       |
| ----- | -------------------------- | ---------------- | ------------------ | ----------------------------------------------------------------------------------------------------------- |
| 2     | Share Responsibility Model | 10/09/2025       | 10/09/2025         | [YouTube Video](https://www.youtube.com/watch?v=tsobAlSg19g&list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i&index=150) |
| 3     | IAM                        | 10/10/2025       | 10/10/2025         | tiếp tục                                                                                                  |
| 4     | Cognito                    | 10/11/2025       | 10/11/2025         | tiếp tục                                                                                                  |
| 5     | AWS Organizations, SSO     | 10/12/2025       | 10/12/2025         | tiếp tục                                                                                                  |
| 6     | KMS, ECS, EKS              | 10/13/2025       | 10/13/2025         | tiếp tục                                                                                                  |

### Kết quả đạt được:

#### **AWS Identity and Access Management (IAM)**

* Kiểm soát identities và permissions cho users, groups, và roles.
* Nguyên tắc cốt lõi: **Least Privilege** – chỉ cấp quyền cần thiết.
* Tránh sử dụng **root account**; sử dụng IAM roles/users thay thế.
* Permissions được định nghĩa qua IAM policies (managed, custom, inline).

#### **Amazon Cognito – Xác thực người dùng cho ứng dụng**

* Cung cấp user sign-up, sign-in, và identity management cho web/mobile apps.
* Hỗ trợ MFA, OAuth2, SAML, và OpenID Connect.
* Giúp offload authentication để không phải xây dựng từ đầu.

#### **AWS Organizations – Quản lý Multi-Account**

* Quản lý tập trung cho nhiều tài khoản AWS.
* Cho phép kiểm soát chi phí, consolidated billing, và security boundaries.
* Sử dụng **Service Control Policies (SCPs)** để enforce permissions toàn tổ chức.

#### **AWS Identity Center (AWS SSO)**

* Đơn giản hóa đăng nhập với **single sign-on** qua các accounts và applications.
* Cho phép gán roles cho users từ internal hoặc external directories.
* Giảm phụ thuộc vào IAM users và long-term access keys.

#### **AWS Key Management Service (KMS)**

* Tạo và quản lý encryption keys cho các dịch vụ AWS.
* Tích hợp với S3, EBS, RDS, DynamoDB, Lambda.
* Cung cấp auditing qua CloudTrail cho key usage.
* Hỗ trợ cả AWS-managed và customer-managed keys.
