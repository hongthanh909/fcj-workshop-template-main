---
title: "Sự kiện 4"
date: 2025-01-01
weight: 4
chapter: false
pre: " <b> 4.4. </b> "
---
# Báo cáo tổng kết: "AWS Well-Architected Security Pillar Workshop"

### Mục tiêu sự kiện

- Hiểu về Security Pillar trong AWS Well-Architected Framework
- Tìm hiểu 5 trụ cột bảo mật cloud cốt lõi với demo thực tế
- Có được insights từ các use cases thực tế và bài học từ doanh nghiệp Việt Nam

### Thông tin sự kiện

**Tên sự kiện:** AWS Well-Architected Security Pillar Workshop

**Thời gian:** Thứ Sáu, 29 tháng 11, 2025 | 8:30 AM – 12:00 PM

**Địa điểm:** Văn phòng AWS Vietnam, Tòa nhà Bitexco Financial Tower, 2 Hải Triều, Quận 1, TP.HCM

**Vai trò:** Người tham dự

### Diễn giả

- **Tran Toan Cong Ly** – *Khai mạc & Nền tảng bảo mật*
- **Hoang Anh** – *Kiến trúc IAM hiện đại*
- **Tran Duc Anh, Nguyen Tuan Thinh, Nguyen Do Thanh Dat** – *Giám sát & Phát hiện liên tục*
- **Hoang Kha** – *Bảo mật hạ tầng & workload*
- **Thinh Lam, Viet Nguyen** – *Mã hóa, quản lý key & secrets*
- **Mende Grabski (Long), Tinh Truong** – *Playbook & Tự động hóa phản ứng sự cố*

---

# Chương trình

| Thời gian           | Chủ đề                                 | Diễn giả                                           |
| -------------------- | ----------------------------------------- | ---------------------------------------------------- |
| 8:30 – 8:50 AM      | Khai mạc & Nền tảng bảo mật          | Tran Toan Cong Ly                                    |
| 8:50 – 9:30 AM      | Trụ cột 1: Identity & Access Management | Hoang Anh                                            |
| 9:30 – 9:55 AM      | Trụ cột 2: Detection                    | Tran Duc Anh, Nguyen Tuan Thinh, Nguyen Do Thanh Dat |
| 9:55 – 10:10 AM     | Coffee Break                              | -                                                    |
| 10:10 – 10:40 AM    | Trụ cột 3: Infrastructure Protection    | Hoang Kha                                            |
| 10:40 – 11:10 AM    | Trụ cột 4: Data Protection              | Thinh Lam, Viet Nguyen                               |
| 11:10 – 11:40 AM    | Trụ cột 5: Incident Response            | Mende Grabski (Long), Tinh Truong                    |
| 11:40 AM – 12:00 PM | Tổng kết & Q&A                          | Tất cả                                             |

---

# Nội dung nổi bật

## Khai mạc & Nền tảng bảo mật (8:30 – 8:50 AM)

**Diễn giả:** Tran Toan Cong Ly

### Vai trò Security Pillar trong Well-Architected Framework

- Các nguyên tắc cốt lõi:
  - Least Privilege (quyền tối thiểu)
  - Zero Trust (không tin cậy mặc định)
  - Defense in Depth (phòng thủ nhiều lớp)
- Mô hình **AWS Shared Responsibility**
- Những mối đe dọa phổ biến trong môi trường cloud tại Việt Nam

---

## Trụ cột 1 — Identity & Access Management (IAM)

### 8:50 – 9:30 AM — Kiến trúc IAM hiện đại

**Diễn giả:** Hoang Anh

### Kiến thức nền tảng về IAM

- Users, Roles, Policies – hạn chế sử dụng **long-term credentials**
- **IAM Identity Center**:
  - Đăng nhập SSO
  - Quản lý permission sets
- **Service Control Policies (SCP)** & permission boundaries cho mô hình multi-account
- MFA, xoay vòng credentials, sử dụng **Access Analyzer**

### Mini Demo

- Kiểm tra và xác thực IAM Policy
- Mô phỏng các kịch bản truy cập khác nhau

---

## Trụ cột 2 — Detection

### 9:30 – 9:55 AM — Giám sát & Phát hiện liên tục

**Diễn giả:** Tran Duc Anh, Nguyen Tuan Thinh, Nguyen Do Thanh Dat

### Dịch vụ giám sát chính

- **AWS CloudTrail**: ghi log ở cấp độ organization
- **Amazon GuardDuty**: phát hiện mối đe dọa
- **AWS Security Hub**: tổng hợp và chuẩn hóa phát hiện bảo mật

### Hệ thống logging toàn diện

- **VPC Flow Logs**
- Log truy cập ALB / S3
- Thiết lập cảnh báo & tự động hóa với **EventBridge**
- Triển khai mô hình *Detection-as-Code* (hạ tầng + rule giám sát)

---

## Trụ cột 3 — Infrastructure Protection

### 10:10 – 10:40 AM — Bảo mật hạ tầng & workload

**Diễn giả:** Hoang Kha

### Bảo mật mạng

- Chiến lược phân đoạn VPC
- Thiết kế subnet public và private hợp lý
- So sánh và ứng dụng **Security Groups** vs **NACLs**

### Dịch vụ bảo vệ

- **AWS WAF** (bảo vệ ứng dụng web)
- **AWS Shield** (bảo vệ DDoS)
- **AWS Network Firewall**

### Bảo vệ workload

- Nguyên tắc bảo mật cho EC2
- Bảo mật container trên ECS/EKS

---

## Trụ cột 4 — Data Protection

### 10:40 – 11:10 AM — Mã hóa, quản lý key & secrets

**Diễn giả:** Thinh Lam, Viet Nguyen

### Quản lý key

- **AWS KMS**: key policies, cấp quyền (grants), xoay vòng key
- Mã hóa dữ liệu at-rest: S3, EBS, RDS, DynamoDB
- Mã hóa in-transit với TLS/SSL

### Quản lý secrets

- **AWS Secrets Manager**
- **AWS Systems Manager Parameter Store**
- Mô hình xoay vòng secrets và best practices

### Quản trị dữ liệu

- Xây dựng chiến lược phân loại dữ liệu
- Thiết lập guardrails và chính sách truy cập phù hợp

---

## Trụ cột 5 — Incident Response

### 11:10 – 11:40 AM — Playbook & Tự động hóa phản ứng sự cố

**Diễn giả:** Mende Grabski (Long), Tinh Truong

### Vòng đời Incident Response

1. Chuẩn bị (Preparation)
2. Phát hiện & Phân tích (Detection & Analysis)
3. Cô lập & Kiểm soát (Containment)
4. Xử lý & Khôi phục (Eradication & Recovery)
5. Rút kinh nghiệm (Post-Incident Activity)

### IR Playbooks tiêu biểu

- Xử lý khi IAM key bị lộ/compromise
- Khắc phục S3 bucket bị public ngoài ý muốn
- Phản ứng khi phát hiện malware trên EC2

### Tự động hóa

- Tự động snapshot & thu thập evidence
- Cô lập instance theo quy trình chuẩn
- Kịch bản auto-response với **Lambda** / **Step Functions**

---

## Tổng kết & Q&A (11:40 AM – 12:00 PM)

### Tóm tắt 5 trụ cột bảo mật

1. Identity & Access Management
2. Detection
3. Infrastructure Protection
4. Data Protection
5. Incident Response

### Các "bẫy" thường gặp

- Vấn đề bảo mật phổ biến tại doanh nghiệp Việt
- Case study & bài học rút ra từ sự cố thực tế

### Lộ trình học & nâng cao

- **AWS Certified Security – Specialty**
- **AWS Certified Solutions Architect – Professional**
- Tài nguyên & kênh học liên tục về bảo mật trên AWS

---

# Bài học rút ra

## Nền tảng bảo mật

- Bảo mật trên cloud là **trách nhiệm chia sẻ** giữa AWS và khách hàng
- Cần thiết kế theo mô hình **defense in depth** – nhiều lớp bảo vệ
- Áp dụng **Zero Trust**:
  - Không tin cậy mặc định
  - Chỉ cấp quyền tối thiểu
  - Luôn giả định có thể bị breach

## Thực hành IAM

- Tránh sử dụng **long-term access keys**, ưu tiên temporary credentials
- Áp dụng MFA cho toàn bộ người dùng tương tác trực tiếp
- Dùng **IAM roles** cho ứng dụng và dịch vụ
- Thường xuyên rà soát quyền với **Access Analyzer**

## Giám sát & Phát hiện

- Bật **CloudTrail** ở cấp organization ngay từ đầu
- Kích hoạt **GuardDuty** để phát hiện mối đe dọa liên tục
- Dùng **Security Hub** làm nơi tổng hợp findings bảo mật
- Tự động hóa quy trình phản ứng với sự kiện bảo mật

## Bảo vệ hạ tầng

- Thiết kế VPC được phân đoạn rõ ràng theo zone/ứng dụng/mức độ nhạy cảm
- Dùng **Security Groups** như lớp firewall chính
- Bảo vệ ứng dụng web bằng **AWS WAF**
- Chọn đúng công cụ bảo vệ phù hợp cho từng loại workload

## Bảo vệ dữ liệu

- Luôn mã hóa dữ liệu **at rest** và **in transit**
- Dùng **KMS** để tập trung quản lý và audit key
- Xoay vòng secrets định kỳ với **Secrets Manager**
- Phân loại dữ liệu để gắn với chính sách truy cập tương ứng

## Ứng phó sự cố

- Xây dựng **IR playbook** trước khi sự cố xảy ra
- Tự động hóa tối đa các bước cô lập và thu thập chứng cứ
- Thường xuyên diễn tập quy trình xử lý sự cố
- Học từ mỗi incident để cải thiện quy trình và kiến trúc

## Bối cảnh doanh nghiệp Việt Nam

- Những khó khăn đặc thù về nhân sự, ngân sách và nhận thức bảo mật
- Yêu cầu tuân thủ (như **PDPA** và quy định nội địa)
- Cách triển khai bảo mật **tối ưu chi phí** nhưng vẫn hiệu quả trên AWS

---

# Trải nghiệm sự kiện

Tham gia workshop **"AWS Well-Architected Security Pillar"** là một trải nghiệm vô giá, cung cấp hiểu biết toàn diện về các best practices bảo mật cloud. Workshop đã bao quát cả năm trụ cột bảo mật với các demo thực tế và ví dụ từ doanh nghiệp Việt Nam.

Các demo thực hành và case studies đã làm cho các khái niệm trở nên cụ thể và có thể áp dụng ngay vào các dự án thực tế. Việc học về mô hình shared responsibility và chiến lược defense-in-depth sẽ cải thiện đáng kể cách tôi tiếp cận bảo mật trong kiến trúc cloud.

### Hình ảnh sự kiện

<!-- Thêm hình ảnh tại đây -->
