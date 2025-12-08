---
title: "Week 5 Worklog"
date: 2025-10-09
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---
### Tasks to be carried out this week:

### **Week 5 Objectives**

* Understand IAM basics: users, roles, and permission policies.
* Learn how Amazon Cognito handles app user authentication.
* Get familiar with AWS Organizations for multi-account management.
* Use AWS Identity Center (SSO) for centralized access control.
* Understand KMS for encryption key management.
* Distinguish responsibility levels across AWS services.
* Grasp the Shared Responsibility Model and the shift toward managed services.

| Day | Task                       | Start Date | Completion Date | Reference Material                                                                                          |
| --- | -------------------------- | ---------- | --------------- | ----------------------------------------------------------------------------------------------------------- |
| 2   | Share Responsibility Model | 10/09/2025 | 10/09/2025      | [YouTube Video](https://www.youtube.com/watch?v=tsobAlSg19g&list=PLahN4TLWtox2a3vElknwzU_urND8hLn1i&index=150) |
| 3   | IAM                        | 10/10/2025 | 10/10/2025      | cont                                                                                                        |
| 4   | Cognito                    | 10/11/2025 | 10/11/2025      | cont                                                                                                        |
| 5   | AWS Organizations, SSO     | 10/12/2025 | 10/12/2025      | cont                                                                                                        |
| 6   | KMS, ECS, EKS              | 10/13/2025 | 10/13/2025      | cont                                                                                                        |

### Understood what AWS is and mastered the basic service groups:

* **AWS Identity and Access Management (IAM)**
* **Amazon Cognito – User Authentication for Applications**
* **AWS Organizations – Multi-Account Management**
* **AWS Identity Center (AWS SSO)**
* **AWS Key Management Service (KMS) – Encryption Key Management**
* **Service Responsibility Levels: Infrastructure / Container / Fully Managed**
* **Summary of Shared Responsibility and Trend Toward Managed Services**

#### **AWS Identity and Access Management (IAM)**

* Controls identities and permissions for users, groups, and roles.
* Core principle: **Least Privilege** – grant only the needed permissions.
* Avoid using the  **root account** ; use IAM roles/users instead.
* Permissions are defined through IAM policies (managed, custom, inline).

#### **Amazon Cognito – User Authentication for Applications**

* Provides user sign-up, sign-in, and identity management for web/mobile apps.
* Supports MFA, OAuth2, SAML, and OpenID Connect.
* Helps offload authentication so you don’t build it from scratch.

#### **AWS Organizations – Multi-Account Management**

* Centralized management for multiple AWS accounts.
* Enables cost control, consolidated billing, and security boundaries.
* Uses **Service Control Policies (SCPs)** to enforce organization-wide permissions.

#### **AWS Identity Center (AWS SSO)**

* Simplifies login with **single sign-on** across accounts and applications.
* Allows assignment of roles to users from internal or external directories.
* Reduces reliance on IAM users and long-term access keys.

#### **AWS Key Management Service (KMS)**

* Creates and manages encryption keys for AWS services.
* Integrates with S3, EBS, RDS, DynamoDB, Lambda.
* Provides auditing via CloudTrail for key usage.
* Supports both AWS-managed and customer-managed keys.

#### **Service Responsibility Levels**

* **Infrastructure services** ( EC2): Customer manages OS, patching, network config.
* **Container services** (ECS/EKS): AWS manages infra; customer manages containers.
* **Fully managed services** (S3, DynamoDB, Lambda): AWS handles almost everything; customer focuses on data.
