---
title: "Worklog Tuần 9"
date: 2025-11-06
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---
### Mục tiêu Tuần 9:

Xây dựng AWS networking (VPC, EC2, RDS) và thực hiện database migrations với DMS/SCT.
Tạo data pipelines (S3 → Glue → Athena → QuickSight) và ETL processing với DynamoDB, DataBrew, EMR.
Xây dựng Redshift data warehouse và interactive BI dashboards.

### Các công việc trong tuần:

| Ngày | Công việc                                                            | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| ---- | -------------------------------------------------------------------- | ------------ | --------------- | ------------------ |
| 2    | VPC, EC2, RDS Networking + EC2 Fleet Manager                         | 11/06/2025   | 11/06/2025      | tiếp tục           |
| 3    | Database Migration (DMS/SCT) + Migration Troubleshooting             | 11/07/2025   | 11/07/2025      | tiếp tục           |
| 4    | S3 Data Pipeline: S3 → Glue → Athena → QuickSight                    | 11/08/2025   | 11/08/2025      | tiếp tục           |
| 5    | DynamoDB + Cloud9, DataBrew & EMR ETL Processing                     | 11/09/2025   | 11/09/2025      | tiếp tục           |
| 6    | Redshift Data Warehouse + Dashboards (ETL, Analytics, Visualization) | 11/10/2025   | 11/10/2025      | tiếp tục           |

### **Kết quả đạt được Tuần 9**

* Xây dựng AWS networking infrastructure (VPC, subnets, routing, SGs, EC2, RDS).
* Quản lý EC2 instances sử dụng Fleet Manager không cần SSH/RDP.
* Hoàn thành end-to-end database migrations sử dụng DMS & SCT.
* Giải quyết migration issues qua logs và event troubleshooting.
* Tạo data pipeline với S3, Glue, Athena, và QuickSight.
* Khám phá DynamoDB và thực hiện backup/restore operations.
* Xử lý large datasets sử dụng Cloud9, DataBrew, và EMR.
* Xây dựng và phân tích Redshift data warehouse.
* Tạo interactive BI dashboards.

### **Networking + Compute Setup**

* Tạo VPC, subnets, và route tables
* Tạo Security Groups cho EC2 và RDS
* Launch EC2 instance
* Launch RDS database
* Sử dụng Fleet Manager để quản lý EC2

### **Database Migration**

* SQL Server → MySQL migration
* Oracle → MySQL migration
* Schema Conversion (SCT)
* Tạo và chạy DMS tasks và endpoints
* Troubleshoot migration logs và events

### **S3 → Glue → Athena → QuickSight Pipeline**

* Tạo S3 bucket
* Tạo Glue Crawler và Data Catalog
* Query data sử dụng Athena
* Xây dựng dashboards trong QuickSight

### **Redshift Data Warehouse**

* Load data vào Redshift
* Transform data với Glue
* Phân tích với Athena hoặc Kinesis
* Xây dựng interactive dashboards
