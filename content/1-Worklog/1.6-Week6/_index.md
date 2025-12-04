---
title: "Week 6 Worklog"
date: 2025-01-01
weight: 1
chapter: false
pre: " <b> 1.6. </b> "
---
## Week 6 Objectives

* Connect and get acquainted with members of First Cloud Journey.
* Understand basic AWS services, how to use the console & CLI.

## Tasks for the Week

| Day | Task                                          | Start Date | Completion Date | Reference Material                                                                                              |
| --- | --------------------------------------------- | ---------- | --------------- | --------------------------------------------------------------------------------------------------------------- |
| 2   | IAM, Account Security & AWS Organizations     | 10/16/2025 | 10/16/2025      | [Reference Document](https://docs.google.com/document/d/1xRjvUkMpaMfi6F1TVUqYi6ZfNGhLoKWlZLEdfTHmjaE/edit?tab=t.0) |
| 3   | Encryption, KMS, ACM & Secrets Manager        | 10/17/2025 | 10/17/2025      | cont                                                                                                            |
| 4   | S3 Storage, Security, Lifecycle & Replication | 10/18/2025 | 10/18/2025      | cont                                                                                                            |
| 5   | VPC & Networking Security (SG, NACL, VPN, DX) | 10/19/2025 | 10/19/2025      | cont                                                                                                            |
| 6   | CloudFront/GA, DR & Backup/Migration          | 10/20/2025 | 10/20/2025      | cont                                                                                                            |

## Week 6 Achievements

### IAM – Identity and Access Management

**Key Points:**

- Root account: setup only, never shared
- IAM Users: represent people/services; IAM Groups organize users
- Policies (JSON): define permissions; follow **Least Privilege**
- Access methods: Console (password + MFA), CLI/SDK (Access Keys)
- IAM Roles: grant permissions to AWS services (EC2, Lambda, etc.)

**Security Tools & Best Practices:**

- **Credentials Report**: lists all users and their credentials status
- **Access Advisor**: shows last-used permissions
- Best practices: no shared users/keys, enforce MFA, use groups, use roles, audit regularly

### AWS Organizations

**Key Points:**

- Manage multiple AWS accounts centrally
- **Management account**: full access; **Member accounts**: controlled accounts
- Consolidated billing: shared cost savings (RI, Savings Plans)
- OUs group accounts for management

**Service Control Policies (SCP):**

- SCP **limits** maximum permissions; does not grant access
- Action allowed only if **IAM Allow + SCP Allow**
- Applies to OUs and member accounts (not management account)

### Encryption, KMS, ACM & Secrets Manager

**Encryption Basics:**

- Encryption in transit → HTTPS/TLS prevents eavesdropping
- Encryption at rest → data is encrypted before being stored

**AWS KMS:**

- Central service for managing encryption keys
- Supports **symmetric** (AES-256) & **asymmetric** keys (RSA/ECC)
- Key types: AWS-Owned, AWS-Managed, Customer-Managed, Imported Keys
- Key Policies control access → required for using the key
- Multi-Region Keys: same logical key replicated across regions
- Copying snapshots across regions → must re-encrypt with a key in the target region

**ACM (AWS Certificate Manager):**

- Issues and manages TLS/SSL certificates
- Free public certificates, automatic renewal
- Integrates with ALB, CloudFront, API Gateway
- Cannot export ACM public certificates to EC2

**Secrets Manager:**

- Secure storage for secrets (passwords, API keys)
- Secrets encrypted using KMS
- Supports automatic rotation (integrated with RDS/Aurora)
- Multi-Region replication for HA/DR

### S3 Storage, Security, Lifecycle & Replication

**S3 Basics:**

- Stores objects in buckets; bucket is regional
- Very high durability (11 nines)
- Security via IAM policies, bucket policies, ACLs

**Storage Classes:**

- Standard, Standard-IA, One-Zone-IA
- Glacier tiers: Instant, Flexible, Deep Archive
- Intelligent-Tiering automatically moves data based on access

**Lifecycle Rules:**

- Transition actions → move objects to cheaper storage
- Expiration actions → delete old objects/versions

**Replication:**

- Requires versioning on both buckets
- CRR = cross-region; SRR = same-region
- Only new objects replicate (old ones use batch replication)
- Delete markers optional; no replication chaining

**S3 Security:**

- Encryption options: SSE-S3, SSE-KMS, SSE-C, Client-Side
- Enforce HTTPS using aws:SecureTransport
- Object Lock (WORM) & Glacier Vault Lock for compliance

### VPC & Networking Security

**VPC Core:**

- VPC = private network; CIDR must not overlap
- Subnets are AZ-specific; public = IGW, private = NAT Gateway
- Route tables define traffic flow
- NAT Gateway → outbound Internet for private subnets
- Bastion Host → SSH into private EC2 instances

**Security Controls:**

- **Security Groups**: instance-level, stateful, allow-only
- **NACLs**: subnet-level, stateless, allow & deny, ordered rules
- VPC Flow Logs → capture traffic for analysis

**Connectivity:**

- **VPC Peering**: private connection between two VPCs (non-transitive)
- **Endpoints**:
  - Gateway Endpoint → S3, DynamoDB (free)
  - Interface Endpoint → PrivateLink (paid), ENI-based
- **Site-to-Site VPN**: IPsec tunnel On-Prem ↔ AWS
- **CloudHub**: connect multiple branch sites via AWS
- **Direct Connect**: dedicated physical link; optional encryption via VPN

**Threat Detection:**

- **GuardDuty**: ML-based threat detection using CloudTrail, Flow Logs, DNS
- **AWS Shield Standard/Advanced**: DDoS protection

### CloudFront, Global Accelerator, DR & Migration

**CloudFront (CDN):**

- Caches content at edge locations using Anycast IP
- Works with S3, ALB, EC2, custom origins
- Supports geo-restrictions, cache invalidation

**Global Accelerator:**

- Speeds up TCP/UDP apps using AWS backbone network
- Provides two global Anycast IPs
- Improves latency and failover across regions

**Disaster Recovery (DR):**

- RPO = acceptable data loss; RTO = acceptable downtime
- DR strategies:
  - Backup & Restore
  - Pilot Light
  - Warm Standby
  - Multi-Region Active/Active
- More resilience = higher cost

**Backup & Migration:**

- **AWS Backup**: centralized backup, cross-account/region, Vault Lock (WORM)
- **Snowball**: offline large-scale data transfer
- **DMS/SMS**: migrate databases & servers to AWS
- **Application Discovery**: analyze on-prem systems before migration
