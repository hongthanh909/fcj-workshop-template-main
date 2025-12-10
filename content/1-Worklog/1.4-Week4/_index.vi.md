---
title: "Worklog Tuần 4"
date: 2025-10-02
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### **Mục tiêu Tuần 4**

* Hiểu Amazon S3 cơ bản và storage classes.
* Học các tính năng S3 nâng cao: static hosting, versioning, Access Points, VPC Endpoints, Glacier.
* Khám phá AWS Snow Family cho di chuyển dữ liệu quy mô lớn.
* Hiểu Storage Gateway cho thiết lập hybrid storage.
* Học chiến lược DR và sử dụng AWS Backup cho automated backups.
* Thực hành qua labs: S3, VM Import/Export, Storage Gateway, FSx, và S3 + CloudFront.

### Các công việc trong tuần:

| Ngày | Công việc                                                                                                                     | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| ---- | ----------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ------------------ |
| 2    | Amazon S3 (Simple Storage Service)                                                                                            | 10/02/2025   | 10/02/2025      | tiếp tục           |
| 3    | Amazon S3 – Tính năng nâng cao                                                                                                | 10/03/2025   | 10/03/2025      | tiếp tục           |
| 4    | AWS Snow Family<br />AWS Storage Gateway<br />Disaster Recovery trên AWS<br />AWS Backup                                      | 10/04/2025   | 10/04/2025      | tiếp tục           |
| 5    | Lab<br />(S3 & Backup)<br />VM Import/Export<br />Storage Gateway<br />FSx (Windows File System)<br />S3 Website & CloudFront | 10/05/2025   | 10/05/2025      | tiếp tục           |
| 6    | Tổng kết kiến thức tuần                                                                                                       | 10/06/2025   | 10/06/2025      |                    |

### Kết quả đạt được Tuần 4:

## Amazon S3 (Simple Storage Service)

### 1. Khái niệm

- Object storage (**WORM**: Write Once, Read Many).
- Update = ghi đè toàn bộ object.

### 2. Tính năng chính

- Lưu trữ không giới hạn, tối đa 5TB/object.
- Replicated qua **3 AZs** → high availability.
- **Durability: 11 nines**, **Availability: 4 nines**.
- Multipart upload, event triggers (Lambda).

### 3. Storage Classes

- **Standard** → truy cập thường xuyên.
- **Standard-IA** → truy cập không thường xuyên.
- **Intelligent-Tiering** → tự động tối ưu chi phí.
- **One Zone-IA** → chi phí thấp, single AZ.
- **Glacier/Deep Archive** → lưu trữ archive.

## AWS Snow Family

- **Snowball**: ~80TB, transfer đến S3/Glacier.
- **Snowball Edge**: ~100TB, với local compute.
- **Snowmobile**: truck cho dữ liệu quy mô PB–EB.

## AWS Storage Gateway

- Hybrid storage: on-prem + AWS.
- **File Gateway**: NFS/SMB → S3 objects.
- **Volume Gateway**: block storage.
- **Tape Gateway**: virtual tape library → S3/Glacier.

## Disaster Recovery (DR) trên AWS

- **RTO** = thời gian recovery.
- **RPO** = mất dữ liệu chấp nhận được.

### Chiến lược DR:

1. **Backup & Restore** (baseline).
2. **Pilot Light** (Active-Standby).
3. **Warm Standby** (Active-Active công suất thấp).
4. **Multi-Site** (Active-Active đầy đủ).

## AWS Backup

- Dịch vụ backup tập trung cho **EC2, EBS, RDS, FSx, EFS**.
- Tính năng: **scheduling, retention, monitoring**.
