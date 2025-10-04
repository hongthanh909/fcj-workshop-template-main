---
title: "Week 1 Worklog"
date: 2025-01-01
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---
---


### Week 1 Objectives:

---

## Tasks for the Week

| Day | Task                                                                                                                                      | Start Date | Completion Date | Reference Material                                                                           |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | -------------------------------------------------------------------------------------------- |
| 1   | - AWS infrastructure<br />- AWS Services<br />  - Create an AWS account <br /> - Create Budget <br /> - Account authentication support | 09/11/2025 | 09/11/2025      |                                                                                              |
| 2   | - What is a VPC                                                                                                                           | 09/12/2025 | 09/12/2025      | [AWS VPC Module 02-01](https://glasp.co/reader?url=https://www.youtube.com/watch?v=O9Ac_vGHquM) |
| 3   | - VPC Security & Multi-VPC features                                                                                                       | 09/13/2025 | 09/13/2025      |                                                                                              |
| 4   | - VPN                                                                                                                                     | 09/14/2025 | 09/15/2025      | [AWS VPN Basics](https://glasp.co/reader?url=https://www.youtube.com/watch?v=CXU8D3kyxIc)       |
| 6   | - Weekly knowledge summary                                                                                                                | 09/15/2025 | 09/15/2025      |                                                                                              |

---

## Achievements

### 1. AWS Infrastructure

- **Region**: Cluster of data centers.
- **Availability Zones (AZs)**: Independent DCs within a region for fault tolerance.
- **Edge Locations**: Used for caching and latency reduction.

### 2. AWS Services & Access

- **Management Tools**: AWS Management Console, AWS CLI.
- **Authentication**: Access Key & Secret Key.
- **Account Setup**:
  - Created AWS account.
  - Enabled MFA devices.
  - Created Admin Group & User.

### 3. Cost & Budget Management

- Cost Budgets
- Usage Budgets
- Reserved Instance (RI) Budgets
- Savings Plans Budgets
- Budget cleanup

### 4. Virtual Private Cloud (VPC)

- **Concepts**: Introduction to AWS Networking, Amazon VPC.
- **Core Components**:
  - Subnets (Public & Private).
  - Availability Zones for high availability.
  - Route Tables & Gateways.
- **Connectivity Options**:
  - VPC Peering & Transit Gateway.
  - VPN & Direct Connect.
- **Additional Features**:
  - Elastic Load Balancing (ELB).
  - Elastic Network Interfaces (ENI).
  - VPC Endpoints.
- **Key Takeaway**:
  VPC is the foundation for secure, scalable, and flexible networking in AWS.

### 5. VPC Security & Multi-VPC Features

- **Security Group (SG)**: Controls inbound/outbound traffic at instance level.
- **Network ACL (NACL)**: Controls traffic at subnet level.
- **VPC Flow Logs**: Capture IP traffic for monitoring and troubleshooting.
- **Multi-VPC Connectivity**:
  - VPC Peering.
  - Transit Gateway.

### 6. VPN & Extra Resources

- **VPN Types**:
  - Site-to-Site VPN.
  - Client-to-Site VPN.
- **Direct Connect**: Dedicated private connection to AWS.
- **Elastic Load Balancing (ELB)**:
  - Health Check.
  - Sticky Sessions (Session Affinity).
  - Application Load Balancer (ALB).
  - Gateway Load Balancer (GLB).
