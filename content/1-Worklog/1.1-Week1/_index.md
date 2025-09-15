---
title: "Week 1 Worklog"
date: 2025-01-01
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---
### Week 1 Objectives:

### Tasks to be carried out this week:

| Day | Task                                                                                                                               | Start Date | Completion Date | Reference Material |
| --- | ---------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ------------------ |
| 1   | - AWS infrastructure<br />- AWS Services<br />- Create an AWS account<br />- Create admin group & admin user<br />- Create Budget | 08/11/2025 | 08/11/2025      |                    |
| 2   | - Create EC2<br />- Instance Purchasing Options                                                                                    | 08/12/2025 | 08/12/2025      |                    |
| 3   | - Create Security Group                                                                                                            | 08/13/2025 | 08/13/2025      |                    |
| 4   | - What is EIP / ENI                                                                                                                | 08/14/2025 | 08/15/2025      |                    |
| 5   | - Weekly knowledge summary                                                                                                         | 08/15/2025 | 08/15/2025      |                    |

### Week 1 Achievements:

* Understood what **AWS infrastructure**

  * Region: cluster of data center
  * AZ
  * Edge Locations
  * **AWS Services**
* - - AWS Management Console
  - - AWS CLI (Command Line Interface)

      Đăng nhập qua **Access Key** và  **Secret Access Key** .

  <img src="/images/CLI.png" alt="AWS CLI Flow" width="400"/>

  AWS SDK (Software Development Kit)
* <img src="/images/SDK.png" alt="AWS SDK Flow" width="400"/>
* **Create Budget**
* - 1. Cost Budgets
    2. Usage budgets
    3. RI Budgets
    4. Saving Plans Budgets

- **Create EC2**

  ![EC2 Architecture](/images/EC2.png)
- EC2 = Elastic Compute Cloud = Infrastructure as a Service
- *It mainly consists in the capability of :*
- Renting virtual machines (EC2)
- Storing data on virtual drives (EBS)
- Distributing load across machines (ELB)
- Scaling the services using an auto-scaling group (ASG)
- **EC2 - User Data**
- That script is only run once at the instance first start, runs with the root user
- **EC2 - Security Groups**

![Security Group Diagram](/images/SG.png)


• Control how traffic is allowed into or out of our EC2( firewall )

• Can be attached to multiple instances

• All inbound traffic is blocked by default

• All outbound traffic is authorised by default

• Security groups only contain allow rules

• Security groups rules can reference by IP or by security group

**Classic Ports to know**

• 22 = SSH (Secure Shell) - log into a Linux instance

• 21 = FTP (File Transfer Protocol) – upload files into a file share

• 22 = SFTP (Secure File Transfer Protocol) – upload files using SSH

• 80 = HTTP – access unsecured websites

• 443 = HTTPS – access secured websites

• 3389 = RDP (Remote Desktop Protocol) – log into a Windows instance

**EC2 Instances Purchasing Options**

• On-Demand Instances – short workload, predictable pricing, pay by second

• Reserved (1 & 3 years)

• Reserved Instances – long workloads

• Convertible Reserved Instances – long workloads with flexible instances

• Savings Plans (1 & 3 years) –commitment to an amount of usage, long workload

• Spot Instances – short workloads, cheap, can lose instances (less reliable)

• Dedicated Hosts – book an entire physical server, control instance placement

• Dedicated Instances – no other customers will share your hardware

• Capacity Reservations – reserve capacity in a specific AZ for any duration

**EC2 - Private vs Public IP (IPv4) & Elastic IP/ ENI**

**EIP**: “An Elastic IP (EIP) provides a static public IPv4 address for an EC2 instance. By default, each AWS account has a quota of 5 EIPs per Region.”

**ENI:** An Elastic Network Interface (ENI) is like a virtual network card that can be attached to an EC2 instance within a VPC. Almost every networking service in the AWS infrastructure relies on ENIs. An ENI can include:

- Primary private IPv4 address
- Secondary private IPv4 addresses
- Elastic IP address (EIP)
- Public IPv4 address
- One or more Security Groups
- MAC address

**Placement Groups :** 

• Cluster

• Spread

• Partition


<p align="center">
  <img src="/images/cluster.png" alt="Cluster Placement Group" width="250"/>
  <img src="/images/spread.png" alt="Spread Placement Group" width="250"/>
  <img src="/images/partition.png" alt="Partition Placement Group" width="250"/>
</p>
