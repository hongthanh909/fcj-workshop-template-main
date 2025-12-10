---
title: "Worklog Tuần 1"
date: 2025-09-11
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---
---

### Mục tiêu Tuần 1:

* Thiết lập tài khoản AWS với MFA, IAM users, và quản lý budget.
* Hiểu hạ tầng AWS (Regions, AZs, Edge Locations) và VPC cơ bản.
* Học VPC security (Security Groups, NACLs, Flow Logs) và kết nối VPN/Direct Connect.

## Các công việc trong tuần

| Ngày | Công việc                                                                                                                                      | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo                                                                           |
| ---- | ---------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | -------------------------------------------------------------------------------------------- |
| 1    | - Hạ tầng AWS<br />- Dịch vụ AWS<br />  - Tạo tài khoản AWS <br /> - Tạo Budget <br /> - Hỗ trợ xác thực tài khoản | 09/11/2025 | 09/11/2025      |                                                                                              |
| 2    | - VPC là gì                                                                                                                           | 09/12/2025 | 09/12/2025      | [AWS VPC Module 02-01](https://glasp.co/reader?url=https://www.youtube.com/watch?v=O9Ac_vGHquM) |
| 3    | - VPC Security & Multi-VPC features                                                                                                       | 09/13/2025 | 09/13/2025      | tiếp tục                                                                                         |
| 4    | - VPN                                                                                                                                     | 09/14/2025 | 09/15/2025      | [AWS VPN Basics](https://glasp.co/reader?url=https://www.youtube.com/watch?v=CXU8D3kyxIc)       |
| 6    | - Tổng kết kiến thức tuần                                                                                                                | 09/15/2025 | 09/15/2025      | tiếp tục                                                                                         |

## Kết quả đạt được

### 1. Hạ tầng AWS

- **Region**: Cụm các data centers.
- **Availability Zones (AZs)**: Các DC độc lập trong một region để chịu lỗi.
- **Edge Locations**: Dùng cho caching và giảm độ trễ.

### 2. Dịch vụ AWS & Truy cập

- **Công cụ quản lý**: AWS Management Console, AWS CLI.
- **Xác thực**: Access Key & Secret Key.
- **Thiết lập tài khoản**:
  - Tạo tài khoản AWS.
  - Bật MFA devices.
  - Tạo Admin Group & User.

### 3. Quản lý Chi phí & Budget

- Cost Budgets
- Usage Budgets
- Reserved Instance (RI) Budgets
- Savings Plans Budgets
- Dọn dẹp budget

### 4. Virtual Private Cloud (VPC)

- **Khái niệm**: Giới thiệu AWS Networking, Amazon VPC.
- **Thành phần chính**:
  - Subnets (Public & Private).
  - Availability Zones cho high availability.
  - Route Tables & Gateways.
- **Tùy chọn kết nối**:
  - VPC Peering & Transit Gateway.
  - VPN & Direct Connect.
- **Tính năng bổ sung**:
  - Elastic Load Balancing (ELB).
  - Elastic Network Interfaces (ENI).
  - VPC Endpoints.
- **Điểm chính**:
  VPC là nền tảng cho networking an toàn, có khả năng mở rộng và linh hoạt trong AWS.

### 5. VPC Security & Multi-VPC Features

- **Security Group (SG)**: Kiểm soát traffic inbound/outbound ở cấp instance.
- **Network ACL (NACL)**: Kiểm soát traffic ở cấp subnet.
- **VPC Flow Logs**: Capture IP traffic để giám sát và troubleshooting.
- **Multi-VPC Connectivity**:
  - VPC Peering.
  - Transit Gateway.

### 6. VPN & Tài nguyên bổ sung

- **Loại VPN**:
  - Site-to-Site VPN.
  - Client-to-Site VPN.
- **Direct Connect**: Kết nối private chuyên dụng đến AWS.
- **Elastic Load Balancing (ELB)**:
  - Health Check.
  - Sticky Sessions (Session Affinity).
  - Application Load Balancer (ALB).
  - Gateway Load Balancer (GLB).
