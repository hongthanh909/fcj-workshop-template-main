---
title: "Worklog Tuần 2"
date: 2025-09-18
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---
### **Mục tiêu Tuần 2:**

* Hiểu và thực hành các thành phần **VPC networking** cốt lõi (subnets, route tables, IGW, NAT, SG, NACLs).
* Xây dựng kinh nghiệm thực hành với **Hybrid DNS & Amazon Route 53** cho phân giải tên cross-network.
* Học cách thiết lập kết nối **VPC Peering** và cấu hình routing giữa các VPCs.
* Khám phá **AWS Transit Gateway** để kết nối nhiều VPCs và quản lý routing tập trung.
* Hoàn thành các labs end-to-end liên quan đến CloudFormation, EC2, DNS resolvers, và kiểm tra kết nối mạng.
* Tổng kết học tập hàng tuần để củng cố các khái niệm AWS networking chính.

### Các công việc trong tuần:

| Ngày | Công việc                | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| ---- | ------------------------ | ------------ | --------------- | ------------------ |
| 2    | VPC (tiếp)               | 09/18/2025   | 09/18/2025      | tiếp tục           |
| 3    | Hydrid DNS & Route 53    | 09/19/2025   | 09/19/2025      | tiếp tục           |
| 4    | VPC Peering              | 09/20/2025   | 09/20/2025      | tiếp tục           |
| 5    | Transit Gateway          | 09/21/2025   | 09/21/2025      | tiếp tục           |
| 6    | Tổng kết kiến thức tuần  | 09/22/2025   | 09/22/2025      |                    |

### 1. VPC (Lab)

- VPN Site-to-Site
- Subnet
- Route Table
- Internet Gateway (IGW)
- NAT Gateway
- Security Group
- Network ACLs
- VPC Resource Map

### 2. Hybrid DNS & Route 53 (Lab10)

- Thiết lập Hybrid DNS
- Tạo Key Pair
- Khởi tạo CloudFormation
- Cấu hình Security Group
- Kết nối đến RDGW
- Thiết lập DNS (Outbound, Resolver, Inbound)
- Kiểm tra kết quả
- Dọn dẹp tài nguyên

### 3. VPC Peering (Lab19)

- Khởi tạo CloudFormation
- Tạo Security Group
- Tạo EC2 Instance
- Cập nhật NACLs
- Tạo Peering Connection
- Cấu hình Route Tables
- Bật Cross-Peer DNS
- Dọn dẹp tài nguyên

### 4. Transit Gateway (Lab20)

- Thiết lập Transit Gateway
- Các bước chuẩn bị
- Tạo Transit Gateway
- Tạo Transit Gateway Attachments
- Tạo Transit Gateway Route Tables
- Thêm Routes vào VPCs
- Dọn dẹp tài nguyên
