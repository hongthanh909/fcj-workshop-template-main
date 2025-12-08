---
title: "Week 9 Worklog"
date: 2025-11-06
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---
### Week 9 Objectives:

Build AWS networking (VPC, EC2, RDS) and perform database migrations with DMS/SCT.
Create data pipelines (S3 → Glue → Athena → QuickSight) and ETL processing with DynamoDB, DataBrew, EMR.
Build Redshift data warehouse and interactive BI dashboards.Tasks to be carried out this week:

| Day | Task                                                                 | Start Date | Completion Date | Reference Material                                                                    |
| --- | -------------------------------------------------------------------- | ---------- | --------------- | ------------------------------------------------------------------------------------- |
| 2   | VPC, EC2, RDS Networking + EC2 Fleet Manager                         | 11/06/2025 | 11/06/2025      | [Summary link](https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i) |
| 3   | Database Migration (DMS/SCT) + Migration Troubleshooting             | 11/07/2025 | 11/07/2025      | [Summary link](https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i) |
| 4   | S3 Data Pipeline: S3 → Glue → Athena → QuickSight                 | 11/08/2025 | 11/08/2025      | [Summary link](https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i) |
| 5   | DynamoDB + Cloud9, DataBrew & EMR ETL Processing                     | 11/09/2025 | 11/09/2025      | [Summary link](https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i) |
| 6   | Redshift Data Warehouse + Dashboards (ETL, Analytics, Visualization) | 11/10/2025 | 11/10/2025      | [Summary link](https://www.youtube.com/playlist?list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i) |

### **Week 9 Achievements**

* Built AWS networking infrastructure (VPC, subnets, routing, SGs, EC2, RDS).
* Managed EC2 instances using Fleet Manager without SSH/RDP.
* Completed end-to-end database migrations using DMS & SCT.
* Resolved migration issues through logs and event troubleshooting.
* Created a data pipeline with S3, Glue, Athena, and QuickSight.
* Explored DynamoDB and performed backup/restore operations.
* Processed large datasets using Cloud9, DataBrew, and EMR.
* Built and analyzed a Redshift data warehouse.
* Created interactive BI dashboards.

### **Networking + Compute Setup**

* Create VPC, subnets, and route tables
* Create Security Groups for EC2 and RDS
* Launch EC2 instance
* Launch RDS database
* Use Fleet Manager to manage EC2

### **Database Migration**

* SQL Server → MySQL migration
* Oracle → MySQL migration
* Schema Conversion (SCT)
* Create and run DMS tasks and endpoints
* Troubleshoot migration logs and events

### **S3 → Glue → Athena → QuickSight Pipeline**

* Create S3 bucket
* Create Glue Crawler and Data Catalog
* Query data using Athena
* Build dashboards in QuickSight

### **NoSQL + ETL Processing**

* Explore DynamoDB
* Perform backup/restore operations
* Set up Cloud9
* Data profiling & cleaning with DataBrew
* Transform data using EMR

### **Redshift Data Warehouse**

* Load data into Redshift
* Transform data with Glue
* Analyze with Athena or Kinesis
* Build interactive dashboards
