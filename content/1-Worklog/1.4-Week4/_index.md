---
title: "Week 4 Worklog"
date: 2025-01-01
weight: 1
chapter: false
pre: " <b> 1.4. </b> "
---
### Week 4 Objectives:

* Connect and get acquainted with members of First Cloud Journey.
* Understand basic AWS services, how to use the console & CLI.

### Tasks to be carried out this week:

| Day | Task                                                                                                                          | Start Date | Completion Date | Reference Material |
| --- | ----------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ------------------ |
| 2   | Amazon S3 (Simple Storage Service)                                                                                            | 08/11/2025 | 08/11/2025      |                    |
| 3   | Amazon S3 – Advanced Features                                                                                                | 08/12/2025 | 08/12/2025      |                    |
| 4   | AWS Snow Family<br />AWS Storage Gateway<br />Disaster Recovery on AWS<br />AWS Backup                                        | 08/13/2025 | 08/13/2025      |                    |
| 5   | Lab<br />(S3 & Backup)<br />VM Import/Export<br />Storage Gateway<br />FSx (Windows File System)<br />S3 Website & CloudFront | 08/14/2025 | 08/15/2025      |                    |
| 6   | Weekly Knowledge Summary                                                                                                      | 08/15/2025 | 08/15/2025      |                    |

### Week 4 Achievements:


# Amazon S3 (Simple Storage Service)

## 1. Concept

- Object storage (**WORM**: Write Once, Read Many).
- Update = overwrite entire object.

## 2. Key Features

- Unlimited storage, max 5TB/object.
- Replicated across **3 AZs** → high availability.
- **Durability: 11 nines**, **Availability: 4 nines**.
- Multipart upload, event triggers (Lambda).

## 3. Bucket & Object

- Objects must be stored in buckets.
- Access via REST API (`PUT`, `GET`).
- Folders are logical only.

## 4. Access Control

- Use **Access Points** for multi-app access.
- Policies: **Identity Policy** + **Resource Policy**.

## 5. Storage Classes

- **Standard** → frequent access.
- **Standard-IA** → infrequent access.
- **Intelligent-Tiering** → auto-optimize cost.
- **One Zone-IA** → low-cost, single AZ.
- **Glacier/Deep Archive** → archival storage.

## 6. Lifecycle Management

- Auto transition objects between storage classes.
- Example: Standard → IA → Glacier.

---

# Amazon S3 – Advanced Features

### Static Website Hosting

- Host static sites/SPAs.
- Enable **CORS** for cross-domain access.

### Access Control

- Prefer **IAM Policies** over ACLs.

### Object & Scaling

- Flat structure, key-based.
- Auto-partition for scale → use random prefixes.

### VPC Endpoint

- Private traffic EC2 ↔ S3 (no Internet).

### Versioning

- Keep object history.
- Protects against accidental delete/overwrite.

### Glacier

- ~20x cheaper for archives.
- Retrieval takes minutes–hours.
- **Object Lock** = immutable storage.

---

# AWS Snow Family

- **Snowball**: ~80TB, transfer to S3/Glacier with Snowball Client.
- **Snowball Edge**: ~100TB, with local compute.
- **Snowmobile**: truck for PB–EB scale data.
- Use case: very large data, slow Internet, no Direct Connect.

---

# AWS Storage Gateway

- Hybrid storage: on-prem + AWS.
- **File Gateway**: NFS/SMB → S3 objects.
- **Volume Gateway**: block storage (Stored Volumes for DR, Cached Volumes for cost).
- **Tape Gateway**: virtual tape library → S3/Glacier.

---

# Disaster Recovery (DR) on AWS

- **RTO** = recovery time.
- **RPO** = acceptable data loss.

### DR Strategies:

1. **Backup & Restore** (baseline).
2. **Pilot Light** (Active-Standby).
3. **Warm Standby** (low-capacity Active-Active).
4. **Multi-Site** (full Active-Active).

---

# AWS Backup

- Centralized backup service for **EC2, EBS, RDS, FSx, EFS**.
- Features: **scheduling, retention, monitoring**.
- Important: retention config to control cost.

---

# LABS

## 🔹 Module 04 – Lab 13: S3 & Backup

- Lab13-02.1: Create S3 Bucket
- Lab13-02.2: Deploy Infrastructure
- Lab13-03: Create Backup Plan
- Lab13-04: Set up Notifications
- Lab13-05: Test Restore
- Lab13-06: Clean up Resources

---

## 🔹 Module 04 – Lab 14: VM Import/Export

- Lab14-01: VMware Workstation
- Lab14-02.1: Export VM from On-premises
- Lab14-02.2: Upload VM to AWS
- Lab14-02.3: Import VM to AWS
- Lab14-02.4: Deploy Instance from AMI
- Lab14-03.1: Setup S3 Bucket ACL
- Lab14-03.2: Export VM from Instance
- Lab14-05: Resource Cleanup

---

## 🔹 Module 04 – Lab 24: Storage Gateway

- Lab24-2.1: Create Storage Gateway
- Lab24-2.2: Create File Shares
- Lab24-2.3: Mount File Shares on On-prem Server
- Lab24-3: Clean up Resources

---

## 🔹 Module 04 – Lab 25: FSx (Windows File System)

- Lab25-2.2: Create SSD Multi-AZ File System
- Lab25-2.3: Create HDD Multi-AZ File System
- Lab25-3: Create New File Shares
- Lab25-4: Test Performance
- Lab25-5: Monitor Performance
- Lab25-6: Enable Data Deduplication
- Lab25-7: Enable Shadow Copies
- Lab25-8: Manage User Sessions & Open Files
- Lab25-9: Enable User Storage Quotas
- Lab25-11: Scale Throughput Capacity
- Lab25-12: Scale Storage Capacity
- Lab25-13: Delete Environment

---

## 🔹 Module 04 – Lab 57: S3 Website & CloudFront

- Lab57-2.1: Create S3 Bucket
- Lab57-2.2: Load Data
- Lab57-3: Enable Static Website Feature
- Lab57-4: Configure Public Access
- Lab57-5: Configure Public Objects
- Lab57-6: Test Website
- Lab57-7.1: Block All Public Access
- Lab57-7.2: Configure Amazon CloudFront
- Lab57-7.3: Test Amazon CloudFront
- Lab57-8: Bucket Versioning
- Lab57-9: Move Objects
- Lab57-10: Replication Object Multi-Region
- Lab57-11: Clean up Resources
