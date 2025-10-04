---
title: "Week 3 Worklog"
date: 2025-01-01
weight: 1
chapter: false
pre: " <b> 1.3. </b> "
---

### Week 3 Objectives:

* Connect and get acquainted with members of First Cloud Journey.
* Understand basic AWS services, how to use the console & CLI.

### Tasks to be carried out this week:

| Day | Task                                                                | Start Date | Completion Date | Reference Material                                                              |
| --- | ------------------------------------------------------------------- | ---------- | --------------- | ------------------------------------------------------------------------------- |
| 2   | Compute VM on AWS                                                   | 08/11/2025 | 08/11/2025      |                                                                                 |
| 3   | EC2 Introduction                                                    | 08/12/2025 | 08/12/2025      | [https://cloudjourney.awsstudygroup.com/](https://cloudjourney.awsstudygroup.com/) |
| 4   | EC2 (Lab)<br />- EC2 & Infrastructure<br />- Storage Gateway      | 08/13/2025 | 08/13/2025      | [https://cloudjourney.awsstudygroup.com/](https://cloudjourney.awsstudygroup.com/) |
| 5   | Lab (cons)<br />-S3 & Static Website<br />-S3 Security & CloudFront | 08/14/2025 | 08/15/2025      | [https://cloudjourney.awsstudygroup.com/](https://cloudjourney.awsstudygroup.com/) |
| 6   | Weekly knowledge summary                                            | 08/15/2025 | 08/15/2025      | [https://cloudjourney.awsstudygroup.com/](https://cloudjourney.awsstudygroup.com/) |

### Week 3 Objectives:1.

### EC2 Basics

- Virtual servers on AWS.
- **Fast launch**, **flexible config**, **lower cost over time**.
- **Elasticity**: scale up/down with demand.

### 2. Instance Types & CPU

- **Intel**: high perf, costly.
- **AMD**: ~10% cheaper.
- **Graviton (ARM)**: up to 40% cheaper, great perf/price (Linux).

### 3. AMI

- Template for EC2.
- Includes: **OS (root vol), permissions, block device mapping**.
- Types: AWS, Marketplace, Custom (Golden Image).

### 4. Snapshot & Backup

- First = full, later = incremental.
- Stored in **S3**.

### 5. Key Pair

- **Linux**: SSH with private key.
- **Windows**: decrypt password via AWS Console → RDP.

### 6. Storage

- **EBS**: persistent, replicated (SSD/HDD), supports Multi-Attach.
- **Instance Store**: NVMe, ultra-fast but ephemeral (cache, logs, swap).

### 7. User Data

- Script runs once at launch.
- Auto config (install apps, start services).

### 8. Meta Data

- Instance info via:

### 9. Other Services

- **Auto Scaling** → adjust instances by demand.
- **EFS** → shared file system.
- **FSx** → managed Windows/Lustre storage.
- **Lightsail** → simple VPS.
- **MGN** → migration service.

### EC2 (Lab)

### 1. EC2 & Infrastructure

* **Lab13-02.1** – Create S3 Bucket (chuẩn bị hạ tầng lưu trữ).
* **Lab13-02.2** – Deploy infrastructure.
* **Lab13-03** – Create Backup plan.
* **Lab13-05** – Test Restore.
* **Lab13-06** – Clean up resources.

### 2. Storage Gateway

* **Lab24-01.2** – Create EC2 for Storage Gateway.
* **Lab24-02.1** – Create Storage Gateway.
* **Lab24-02.2** – Create File Shares.

### 3. S3 & Static Website

* **Lab57-02.1** – Create S3 bucket.
* **Lab57-02.2** – Load data.
* **Lab57-03** – Enable static website feature.
* **Lab57-04** – Configuring public access.
* **Lab57-05** – Configuring public objects.
* **Lab57-06** – Test website.

### 4. S3 Security & CloudFront

* **Lab57-07.1** – Block all public access.
* **Lab57-07.2** – Configure Amazon CloudFront.
* **Lab57-07.3** – Test Amazon CloudFront.
* **Lab57-08** – Bucket Versioning.
* **Lab57-09** – Move objects.
* **Lab57-10** – Replication Object multi-region.
* **Lab57-11** – Clean up resources.
