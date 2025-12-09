---
title: "Event 2"
date: 2025-01-01
weight: 1
chapter: false
pre: " <b> 4.2. </b> "
---
# Summary Report: “DevOps on AWS”

### Event Objectives

* Introduce the fundamentals and benefits of  **Infrastructure as Code (IaC)** .
* Provide an in-depth overview of **AWS CloudFormation** and  **AWS CDK** .
* Offer foundational knowledge on  **Docker** , container images, and workflows.
* Explore AWS container services:  **ECR, ECS, EKS, and App Runner** .
* Present CI/CD implementation approaches and comparisons between DevOps tools.

### Speakers

* **Bao Huynh** – AWS Community Builder
* **Thinh Nguyen** – AWS Community Builder
* **Vi Tran** – AWS Community Builder

### Key Highlights

#### Transitioning to modern application architecture – Microservices

### 1. Infrastructure as Code (IaC)

* IaC eliminates ClickOps limitations such as slowness, inconsistency, and human errors (shown on  *slide 7* ).
* Key benefits: automation, scalability, reproducibility, and improved team collaboration.
* Helps maintain reliable infrastructure by avoiding manual configuration drift.

### 2. AWS CloudFormation

* AWS’s native IaC tool using **YAML/JSON** templates.
* Key concepts:
  * **Stack** – a unit that groups and manages AWS resources.
  * **Template Anatomy**:
    * `AWSTemplateFormatVersion`
    * `Description`
    * `Parameters` – input values such as KeyPair
    * `Mappings` – e.g., selecting AMI per region
    * `Conditions`
    * **Resources** – required section
    * `Outputs` – values like public IP for cross-stack usage
* Supports **Drift Detection** to identify manual changes outside CloudFormation.

### 3. AWS CDK (Cloud Development Kit)

* An open-source IaC framework using programming languages (TypeScript, Python, Java, Go, C#, etc.).
* Uses the **constructs** model:

  * **L1** – direct 1:1 CloudFormation mapping
  * **L2** – higher-level, developer-friendly with best practices
  * **L3** – complete architectural patterns
* Important CDK CLI commands:

  `cdk init`,
* `cdk bootstrap`,
* `cdk synth`,
* `cdk deploy`,
* `cdk diff`,
* `cdk destroy`,
* `cdk drift`,
* `cdk doctor`,
* `cdk import`.
* CDK synthesizes CloudFormation templates before deployment.

### 4. Docker & Container Fundamentals

* Docker standardizes application packaging across environments.
* **Containers vs VMs** (slide 68): containers are lightweight, fast, and resource-efficient.
* Docker pipeline:  **Dockerfile → Image → Container** ; images stored in registries like  **ECR** .

### 5. Amazon ECR

* AWS’s fully managed private container registry.
* Key features:
  * Image scanning
  * Immutable tags
  * Lifecycle policies
  * Encryption & IAM control

### 6. Amazon ECS

* Fully managed container orchestration service from AWS.
* Two launch types:
  * **Fargate** – serverless, no infrastructure management
  * **EC2** – more control and cost-optimized for long-running tasks
* ECS core components:
  * **Cluster**
  * **Task Definition**
  * **Task**
  * **Service**

### 7. Amazon EKS

* Fully managed **Kubernetes** service on AWS.
* Automates control plane operations, scaling, and upgrades.
* Can run workloads on EC2, Fargate, or Outposts.
* **ECS vs EKS** (slide 89):
  * **ECS** : simpler, deeply AWS integrated, less operational overhead
  * **EKS** : Kubernetes standard, more flexibility, higher complexity

### 8. AWS App Runner

* A quick and managed way to deploy web applications and APIs from GitHub or ECR.
* Provides automatic build, scaling, security, and HTTPS endpoint.
* Suitable for microservices, prototypes, and small-to-medium production workloads.

# Key Takeaways

### IaC Mindset

* Reduce manual console operations; fully embrace IaC for consistency and automation.
* IaC increases deployment reliability and minimizes configuration drift.

### Technical Architecture

* CloudFormation is ideal for precise resource definitions.
* CDK enables reusable patterns, faster development, and code abstraction.
* ECS is best for simplicity; EKS is best for teams requiring full Kubernetes capabilities.

### DevOps & Containers

* Standardize application packaging with Docker.
* Use ECR + ECS/EKS workflows for secure, scalable deployment.
* App Runner is great for fast deployments with minimal DevOps overhead.

# Applying to Work

* Adopt IaC across all projects using CloudFormation or CDK.
* Implement a container pipeline (build → push → deploy).
* Prevent drift by standardizing infrastructure repositories.
* Use **ECS Fargate** for microservices; adopt **EKS** if Kubernetes expertise is required.
* Test **App Runner** for lightweight services and rapid prototyping.

# Event Experience

* The workshop provided a clear transition path from manual operations to modern IaC practices.
* CDK construct levels (L1 → L2 → L3) helped visualize infrastructure abstraction and best practices.
* Live demos on CloudFormation and ECS made the deployment workflow easier to understand.
* Discussions highlighted how IaC and containerization reduce operational burden and improve scalability.

### Hands-on Technical Exposure (Summary)

* Practiced defining infrastructure using  **CloudFormation templates** , including parameters, mappings, conditions, and outputs.
* Used **AWS CDK CLI commands** to generate, synthesize, and deploy stacks, seeing how high-level code becomes CloudFormation templates.
* Explored construct levels (L1–L3) through examples showing increasing abstraction and best practices.
* Gained practical understanding of **ClickOps vs IaC** and how drift detection helps maintain infrastructure consistency.

### Leveraging Modern Tools (Summary)

* Learned how **AWS CDK** enables infrastructure provisioning through programming languages.
* Explored how **Amplify** uses CloudFormation behind the scenes for backend deployments.
* Reviewed Docker workflows and worked with  **ECR, ECS, EKS** , and **App Runner** for container deployments.
* Understood how managed services automate scaling, security, and deployment pipelines.

### Networking and Discussions (Summary)

* Discussed real-world IaC adoption challenges with AWS Community Builders and peers.
* Gained insights into choosing between **ECS and EKS** depending on operational needs and complexity.
* Shared experiences on migrating from manual operations to automated DevOps practices.

### Lessons Learned (Summary)

* IaC increases  **automation, consistency, and reproducibility** , reducing human error.
* CloudFormation provides precise control, while CDK boosts developer productivity with higher-level constructs.
* Choosing the right container service (ECS, EKS, App Runner) impacts scalability and operational effort.
* Effective DevOps combines IaC, CI/CD, containers, and automation to build resilient cloud architectures.

### Event Photos

<img src="/images/event2.1.jpg" alt="Event 2.1" width="450">

<img src="/images/event2.jpg" alt="Event 2" width="450">
