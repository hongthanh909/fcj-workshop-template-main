---
title: "Worklog Tuần 3"
date: 2025-09-25
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---
### Mục tiêu Tuần 3:

* Hiểu EC2 cơ bản: instance types, AMI, snapshots, storage (EBS, Instance Store).
* Thực hành labs với EC2, Storage Gateway, S3 static website, và CloudFront.
* Học Auto Scaling, EFS, FSx, và dịch vụ VM migration.

### Các công việc trong tuần:

| Ngày | Công việc                                                           | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| ---- | ------------------------------------------------------------------- | ------------ | --------------- | ------------------ |
| 2    | Compute VM trên AWS                                                 | 09/25/2025   | 09/25/2025      | [VM on AWS](https://www.youtube.com/watch?v=-t5h4N6vfBs) |
| 3    | Giới thiệu EC2                                                      | 09/26/2025   | 09/26/2025      | [EC2](https://www.youtube.com/watch?v=e7XeKdOVq40) |
| 4    | EC2 (Lab)<br />- EC2 & Infrastructure<br />- Storage Gateway        | 09/27/2025   | 09/27/2025      | tiếp tục |
| 5    | Lab (tiếp)<br />-S3 & Static Website<br />-S3 Security & CloudFront | 09/28/2025   | 09/28/2025      | [S3](https://www.youtube.com/watch?v=_yunukwcAwc) |
| 6    | Tổng kết kiến thức tuần                                             | 09/29/2025   | 09/29/2025      | |

### Kết quả đạt được

### 1. EC2 Cơ bản

- Virtual servers trên AWS.
- **Khởi động nhanh**, **cấu hình linh hoạt**, **chi phí giảm theo thời gian**.
- **Elasticity**: scale up/down theo nhu cầu.

### 2. Instance Types & CPU

- **Intel**: hiệu suất cao, chi phí cao.
- **AMD**: rẻ hơn ~10%.
- **Graviton (ARM)**: rẻ hơn đến 40%, hiệu suất/giá tốt (Linux).

### 3. AMI

- Template cho EC2.
- Bao gồm: **OS (root vol), permissions, block device mapping**.
- Loại: AWS, Marketplace, Custom (Golden Image).

### 4. Snapshot & Backup

- Lần đầu = full, sau đó = incremental.
- Lưu trữ trong **S3**.

### 5. Storage

- **EBS**: persistent, replicated (SSD/HDD), hỗ trợ Multi-Attach.
- **Instance Store**: NVMe, siêu nhanh nhưng ephemeral (cache, logs, swap).

### EC2 (Lab)

### 1. EC2 & Infrastructure

* **Lab13-02.1** – Tạo S3 Bucket.
* **Lab13-02.2** – Deploy infrastructure.
* **Lab13-03** – Tạo Backup plan.
* **Lab13-05** – Test Restore.
* **Lab13-06** – Dọn dẹp tài nguyên.

### 2. Storage Gateway

* **Lab24-01.2** – Tạo EC2 cho Storage Gateway.
* **Lab24-02.1** – Tạo Storage Gateway.
* **Lab24-02.2** – Tạo File Shares.

### 3. S3 & Static Website

* **Lab57-02.1** – Tạo S3 bucket.
* **Lab57-02.2** – Load data.
* **Lab57-03** – Bật tính năng static website.
* **Lab57-04** – Cấu hình public access.
* **Lab57-05** – Cấu hình public objects.
* **Lab57-06** – Test website.

### 4. S3 Security & CloudFront

* **Lab57-07.1** – Block all public access.
* **Lab57-07.2** – Cấu hình Amazon CloudFront.
* **Lab57-07.3** – Test Amazon CloudFront.
* **Lab57-08** – Bucket Versioning.
* **Lab57-09** – Move objects.
* **Lab57-10** – Replication Object multi-region.
* **Lab57-11** – Dọn dẹp tài nguyên.
