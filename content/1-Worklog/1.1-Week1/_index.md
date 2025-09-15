---
---
title: "Week 1 Worklog"
date: 2025-01-01
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---

## Week 1 Objectives

- Hiểu khái niệm cơ bản về **AWS Infrastructure** và **AWS Services**.  
- Tạo **AWS Account**, **Admin Group**, **Admin User**.  
- Quản lý chi phí bằng **Budgets**.  
- Làm quen với **EC2** và các tùy chọn triển khai.  
- Tìm hiểu về **Security Groups**, **EIP**, **ENI**, và **Placement Groups**.  

---

## Tasks

| Day | Task                                                                                  | Start Date | Completion Date |
| --- | ------------------------------------------------------------------------------------- | ---------- | --------------- |
| 1   | - AWS Infrastructure<br>- AWS Services<br>- Create AWS Account<br>- Admin Group/User<br>- Create Budget | 08/11/2025 | 08/11/2025      |
| 2   | - Create EC2<br>- Instance Purchasing Options                                         | 08/12/2025 | 08/12/2025      |
| 3   | - Create Security Group                                                               | 08/13/2025 | 08/13/2025      |
| 4   | - EIP / ENI concepts                                                                  | 08/14/2025 | 08/15/2025      |
| 5   | - Weekly knowledge summary                                                            | 08/15/2025 | 08/15/2025      |

---

## Week 1 Achievements

### 1. AWS Infrastructure

- **Region**: cụm nhiều Data Centers.  
- **Availability Zone (AZ)**: đơn vị nhỏ hơn Region.  
- **Edge Location**: điểm hiện diện biên để tăng tốc độ phân phối nội dung.  

### 2. AWS Services & Access Methods

- **AWS Management Console**: giao diện web để quản lý dịch vụ.  
- **AWS CLI (Command Line Interface)**:  
  - Đăng nhập bằng **Access Key** và **Secret Access Key**.  
  - Dùng để quản lý tài nguyên bằng lệnh.  
  <p align="center">
    <img src="/images/CLI.png" alt="AWS CLI Flow" width="400"/>
  </p>

- **AWS SDK (Software Development Kit)**:  
  - Dùng để tương tác AWS qua code.  
  <p align="center">
    <img src="/images/SDK.png" alt="AWS SDK Flow" width="400"/>
  </p>

---

### 3. AWS Budgets

- **Cost Budgets**  
- **Usage Budgets**  
- **RI (Reserved Instance) Budgets**  
- **Savings Plans Budgets**  

---

### 4. Amazon EC2

#### 4.1 Tổng quan
- **EC2 (Elastic Compute Cloud)**: dịch vụ IaaS cho phép:  
  - Thuê máy ảo.  
  - Lưu trữ dữ liệu trên EBS.  
  - Phân phối tải qua ELB.  
  - Tự động mở rộng với Auto Scaling Group (ASG).  

![EC2 Architecture](/images/EC2.png)

#### 4.2 User Data
- Script khởi chạy **chỉ chạy 1 lần** khi instance start lần đầu.  
- Chạy với quyền **root**.  

#### 4.3 Security Groups
- Firewall kiểm soát inbound/outbound traffic.  
- Có thể gắn nhiều instance.  
- Mặc định:
  - Inbound: **block all**.  
  - Outbound: **allow all**.  
- Chỉ có **allow rules**, không có deny.  
- Rule có thể tham chiếu theo **IP** hoặc **Security Group**.  

![Security Group Diagram](/images/SG.png)

**Classic Ports cần nhớ:**
- `22` = SSH (Linux)  
- `21` = FTP  
- `22` = SFTP  
- `80` = HTTP  
- `443` = HTTPS  
- `3389` = RDP (Windows)  

#### 4.4 Instance Purchasing Options
- **On-Demand**: workload ngắn, tính theo giây.  
- **Reserved (1–3 năm)**: workload dài hạn.  
- **Convertible Reserved**: workload dài, flexible instance.  
- **Savings Plans (1–3 năm)**: cam kết usage, tiết kiệm chi phí.  
- **Spot Instances**: workload ngắn, rẻ nhưng không đảm bảo.  
- **Dedicated Hosts**: thuê toàn bộ server vật lý.  
- **Dedicated Instances**: phần cứng riêng, không chia sẻ.  
- **Capacity Reservations**: giữ sẵn capacity trong AZ.  

#### 4.5 Networking: Public/Private IP, EIP, ENI
- **EIP (Elastic IP)**: IPv4 tĩnh, mặc định quota 5 EIP/Region.  
- **ENI (Elastic Network Interface)**: network card ảo, gắn vào instance.  
  - Primary/Secondary private IPv4.  
  - Public IPv4 / Elastic IP.  
  - Security Groups.  
  - MAC address.  

#### 4.6 Placement Groups
- **Cluster**: tập trung instance để giảm latency.  
- **Spread**: phân tán instance trên nhiều phần cứng.  
- **Partition**: chia theo nhóm logical, scale tốt.  

<p align="center">
  <img src="/images/cluster.png" alt="Cluster Placement Group" width="250"/>
  <img src="/images/spread.png" alt="Spread Placement Group" width="250"/>
  <img src="/images/partition.png" alt="Partition Placement Group" width="250"/>
</p>


---
