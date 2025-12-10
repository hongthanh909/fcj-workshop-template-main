---
title: "Event 4"
date: 2025-01-01
weight: 4
chapter: false
pre: " <b> 4.4. </b> "
---
# Summary Report: "AWS Well-Architected Security Pillar Workshop"

### Event Objectives

- Understand the Security Pillar within the AWS Well-Architected Framework
- Learn the 5 core cloud security pillars with practical demonstrations
- Gain insights from real-world use cases and lessons learned from Vietnamese enterprises

### Event Information

**Event Name:** AWS Well-Architected Security Pillar Workshop

**Date & Time:** Friday, November 29, 2025 | 8:30 AM – 12:00 PM

**Location:** AWS Vietnam Office, Bitexco Financial Tower, 2 Hai Trieu St., District 1, HCMC

**Role:** Attendee

### Speakers

- **Tran Toan Cong Ly** – *Opening & Security Foundations*
- **Hoang Anh** – *Modern IAM Architecture*
- **Tran Duc Anh, Nguyen Tuan Thinh, Nguyen Do Thanh Dat** – *Continuous Monitoring & Threat Detection*
- **Hoang Kha** – *Securing Network & Workloads*
- **Thinh Lam, Viet Nguyen** – *Encryption, Key Management & Secrets*
- **Mende Grabski (Long), Tinh Truong** – *Playbooks & Automated Incident Response*

---

# Agenda

| Time                 | Topic                                  | Speaker                                              |
| -------------------- | -------------------------------------- | ---------------------------------------------------- |
| 8:30 – 8:50 AM      | Opening & Security Foundations         | Tran Toan Cong Ly                                    |
| 8:50 – 9:30 AM      | Pillar 1: Identity & Access Management | Hoang Anh                                            |
| 9:30 – 9:55 AM      | Pillar 2: Detection                    | Tran Duc Anh, Nguyen Tuan Thinh, Nguyen Do Thanh Dat |
| 9:55 – 10:10 AM     | Coffee Break                           | -                                                    |
| 10:10 – 10:40 AM    | Pillar 3: Infrastructure Protection    | Hoang Kha                                            |
| 10:40 – 11:10 AM    | Pillar 4: Data Protection              | Thinh Lam, Viet Nguyen                               |
| 11:10 – 11:40 AM    | Pillar 5: Incident Response            | Mende Grabski (Long), Tinh Truong                    |
| 11:40 AM – 12:00 PM | Wrap-Up & Q&A                          | All                                                  |

---

# Key Highlights

## Opening & Security Foundations (8:30 – 8:50 AM)

**Speaker:** Tran Toan Cong Ly

### Role of the Security Pillar in the Well-Architected Framework

- Core principles:
  - Least Privilege
  - Zero Trust
  - Defense in Depth
- The **AWS Shared Responsibility Model**
- Common security threats in the Vietnam cloud landscape

---

## Pillar 1 — Identity & Access Management (IAM)

### 8:50 – 9:30 AM — Modern IAM Architecture

**Speaker:** Hoang Anh

### IAM Fundamentals

- Users, Roles, Policies – minimizing the use of **long-term credentials**
- **IAM Identity Center**:
  - Single Sign-On (SSO)
  - Managing permission sets
- **Service Control Policies (SCPs)** & permission boundaries in multi-account setups
- MFA, credential rotation, and using **Access Analyzer**

### Mini Demo

- Validate and test IAM policies
- Simulate different access scenarios

---

## Pillar 2 — Detection

### 9:30 – 9:55 AM — Continuous Monitoring & Threat Detection

**Speakers:** Tran Duc Anh, Nguyen Tuan Thinh, Nguyen Do Thanh Dat

### Key Monitoring Services

- **AWS CloudTrail**: organization-level auditing and logging
- **Amazon GuardDuty**: intelligent threat detection
- **AWS Security Hub**: consolidated and normalized security findings

### End-to-End Logging

- **VPC Flow Logs**
- ALB and S3 access logs
- Alerts and automated workflows using **EventBridge**
- Implementing *Detection-as-Code* (infrastructure + detection rules)

---

## Pillar 3 — Infrastructure Protection

### 10:10 – 10:40 AM — Securing Network & Workloads

**Speaker:** Hoang Kha

### Network Security

- VPC segmentation strategies
- Proper design of public and private subnets
- Practical use of **Security Groups** vs **NACLs**

### Protection Services

- **AWS WAF** (Web Application Firewall)
- **AWS Shield** (DDoS protection)
- **AWS Network Firewall**

### Workload Protection

- Foundational security best practices for EC2
- Container security on ECS/EKS

---

## Pillar 4 — Data Protection

### 10:40 – 11:10 AM — Encryption, Key Management & Secrets

**Speakers:** Thinh Lam, Viet Nguyen

### Key Management

- **AWS KMS**: key policies, grants, key rotation
- Encryption at rest for: S3, EBS, RDS, DynamoDB
- Encryption in transit using TLS/SSL

### Secrets Management

- **AWS Secrets Manager**
- **AWS Systems Manager Parameter Store**
- Recommended patterns and best practices for rotating secrets

### Data Governance

- Designing data classification strategies
- Defining guardrails and access policies

---

## Pillar 5 — Incident Response

### 11:10 – 11:40 AM — Playbooks & Automated Incident Response

**Speakers:** Mende Grabski (Long), Tinh Truong

### Incident Response Lifecycle

1. Preparation
2. Detection & Analysis
3. Containment
4. Eradication & Recovery
5. Post-Incident Review and improvement

### Sample IR Playbooks

- Responding to compromised IAM access keys
- Remediating unintended public S3 bucket exposure
- Handling malware detection on EC2 instances

### Automation

- Automated snapshots and evidence collection
- Standardized instance isolation workflows
- Auto-response patterns using **Lambda** / **Step Functions**

---

## Wrap-Up & Q&A (11:40 AM – 12:00 PM)

### Recap of the 5 Security Pillars

1. Identity & Access Management
2. Detection
3. Infrastructure Protection
4. Data Protection
5. Incident Response

### Common Pitfalls

- Typical security gaps in Vietnamese organizations
- Case studies and key lessons from real incidents

### Learning Path & Next Steps

- **AWS Certified Security – Specialty**
- **AWS Certified Solutions Architect – Professional**
- Ongoing learning resources and channels for AWS security

---

# Key Takeaways

## Security Foundations

- Cloud security follows a **shared responsibility** between AWS and customers
- Architect solutions using **defense in depth** across multiple layers
- Embrace a **Zero Trust** mindset:
  - Do not trust by default
  - Grant only the minimum required access
  - Operate under an "assume breach" model

## IAM Practices

- Avoid **long-term access keys**; prefer temporary credentials
- Enforce MFA for all human users
- Use **IAM roles** for applications and services
- Perform regular permission reviews with **Access Analyzer**

## Monitoring & Detection

- Enable **CloudTrail** at the organization level from day one
- Turn on **GuardDuty** for continuous threat monitoring
- Use **Security Hub** as the central view of security findings
- Automate security event handling wherever possible

## Infrastructure Protection

- Design VPCs with clear segmentation by zone, application, or sensitivity level
- Use **Security Groups** as the primary network firewall
- Protect web-facing workloads with **AWS WAF**
- Choose appropriate protection tools for each workload type

## Data Protection

- Always encrypt data **at rest** and **in transit**
- Rely on **KMS** for centralized key management and auditing
- Rotate secrets regularly via **Secrets Manager**
- Classify data and enforce matching access policies

## Incident Response

- Build and maintain **IR playbooks** before incidents occur
- Automate key steps like isolation and evidence gathering
- Regularly rehearse incident response processes
- Treat every incident as input to improve architecture and procedures

## Vietnamese Enterprise Context

- Challenges specific to Vietnam: skills, budget, and security awareness
- Compliance drivers (such as **PDPA** and local regulations)
- Strategies to implement strong AWS security in a **cost-effective** way

---

# Event Experience

Participating in the **"AWS Well-Architected Security Pillar Workshop"** was an invaluable experience that provided a comprehensive understanding of cloud security best practices. The workshop covered all five security pillars with practical demonstrations and real-world examples from Vietnamese enterprises.

The hands-on demos and case studies made the concepts tangible and immediately applicable to real projects. Learning about the shared responsibility model and defense-in-depth strategies will significantly improve how I approach security in cloud architectures.

### Event Photos

<!-- Add your photos here -->
