---
title: "Sự kiện 2"
date: 2025-01-01
weight: 1
chapter: false
pre: " <b> 4.2. </b> "
---
# Báo cáo tổng kết: "DevOps trên AWS"

### Mục tiêu sự kiện

* Giới thiệu các nguyên tắc cơ bản và lợi ích của **Infrastructure as Code (IaC)**.
* Cung cấp cái nhìn tổng quan chuyên sâu về **AWS CloudFormation** và **AWS CDK**.
* Cung cấp kiến thức nền tảng về **Docker**, container images, và workflows.
* Khám phá các dịch vụ container AWS: **ECR, ECS, EKS, và App Runner**.
* Trình bày các phương pháp triển khai CI/CD và so sánh giữa các công cụ DevOps.

### Diễn giả

* **Bao Huynh** – AWS Community Builder
* **Thinh Nguyen** – AWS Community Builder
* **Vi Tran** – AWS Community Builder

### Nội dung nổi bật

### 1. Infrastructure as Code (IaC)

* IaC loại bỏ các hạn chế của ClickOps như chậm, không nhất quán, và lỗi con người.
* Lợi ích chính: automation, scalability, reproducibility, và cải thiện team collaboration.
* Giúp duy trì infrastructure đáng tin cậy bằng cách tránh manual configuration drift.

### 2. AWS CloudFormation

* Công cụ IaC native của AWS sử dụng **YAML/JSON** templates.
* Các khái niệm chính:
  * **Stack** – một đơn vị nhóm và quản lý AWS resources.
  * **Template Anatomy**:
    * `AWSTemplateFormatVersion`
    * `Description`
    * `Parameters` – input values như KeyPair
    * `Mappings` – ví dụ, chọn AMI theo region
    * `Conditions`
    * **Resources** – phần bắt buộc
    * `Outputs` – values như public IP cho cross-stack usage
* Hỗ trợ **Drift Detection** để xác định các thay đổi thủ công ngoài CloudFormation.

### 3. AWS CDK (Cloud Development Kit)

* Framework IaC open-source sử dụng programming languages (TypeScript, Python, Java, Go, C#, v.v.).
* Sử dụng mô hình **constructs**:

  * **L1** – direct 1:1 CloudFormation mapping
  * **L2** – higher-level, developer-friendly với best practices
  * **L3** – complete architectural patterns
* Các CDK CLI commands quan trọng:

  `cdk init`, `cdk bootstrap`, `cdk synth`, `cdk deploy`, `cdk diff`, `cdk destroy`, `cdk drift`, `cdk doctor`, `cdk import`.
* CDK synthesizes CloudFormation templates trước khi deployment.

### 4. Docker & Container Fundamentals

* Docker chuẩn hóa application packaging trên các environments.
* **Containers vs VMs**: containers nhẹ, nhanh, và tiết kiệm tài nguyên.
* Docker pipeline: **Dockerfile → Image → Container**; images được lưu trong registries như **ECR**.

### 5. Amazon ECR

* Private container registry được quản lý hoàn toàn của AWS.
* Các tính năng chính:
  * Image scanning
  * Immutable tags
  * Lifecycle policies
  * Encryption & IAM control

### 6. Amazon ECS

* Dịch vụ container orchestration được quản lý hoàn toàn từ AWS.
* Hai launch types:
  * **Fargate** – serverless, không cần quản lý infrastructure
  * **EC2** – nhiều control hơn và cost-optimized cho long-running tasks
* Các ECS core components:
  * **Cluster**
  * **Task Definition**
  * **Task**
  * **Service**

### 7. Amazon EKS

* Dịch vụ **Kubernetes** được quản lý hoàn toàn trên AWS.
* Tự động hóa control plane operations, scaling, và upgrades.
* Có thể chạy workloads trên EC2, Fargate, hoặc Outposts.
* **ECS vs EKS**:
  * **ECS**: đơn giản hơn, tích hợp sâu với AWS, ít operational overhead
  * **EKS**: Kubernetes standard, linh hoạt hơn, phức tạp hơn

### 8. AWS App Runner

* Cách nhanh chóng và được quản lý để deploy web applications và APIs từ GitHub hoặc ECR.
* Cung cấp automatic build, scaling, security, và HTTPS endpoint.
* Phù hợp cho microservices, prototypes, và small-to-medium production workloads.

---

# Bài học rút ra

### Tư duy IaC

* Giảm manual console operations; hoàn toàn embrace IaC cho consistency và automation.
* IaC tăng deployment reliability và giảm thiểu configuration drift.

### Kiến trúc kỹ thuật

* CloudFormation lý tưởng cho precise resource definitions.
* CDK cho phép reusable patterns, faster development, và code abstraction.
* ECS tốt nhất cho simplicity; EKS tốt nhất cho teams cần full Kubernetes capabilities.

### DevOps & Containers

* Chuẩn hóa application packaging với Docker.
* Sử dụng ECR + ECS/EKS workflows cho secure, scalable deployment.
* App Runner tuyệt vời cho fast deployments với minimal DevOps overhead.

---

# Áp dụng vào công việc

* Áp dụng IaC trên tất cả projects sử dụng CloudFormation hoặc CDK.
* Triển khai container pipeline (build → push → deploy).
* Ngăn drift bằng cách chuẩn hóa infrastructure repositories.
* Sử dụng **ECS Fargate** cho microservices; áp dụng **EKS** nếu cần Kubernetes expertise.
* Test **App Runner** cho lightweight services và rapid prototyping.

---

# Trải nghiệm sự kiện

* Workshop cung cấp con đường chuyển đổi rõ ràng từ manual operations sang modern IaC practices.
* CDK construct levels (L1 → L2 → L3) giúp hình dung infrastructure abstraction và best practices.
* Live demos trên CloudFormation và ECS làm cho deployment workflow dễ hiểu hơn.
* Các discussions highlight cách IaC và containerization giảm operational burden và cải thiện scalability.

### Trải nghiệm kỹ thuật thực hành (Tóm tắt)

* Thực hành định nghĩa infrastructure sử dụng **CloudFormation templates**, bao gồm parameters, mappings, conditions, và outputs.
* Sử dụng **AWS CDK CLI commands** để generate, synthesize, và deploy stacks, thấy cách high-level code trở thành CloudFormation templates.
* Khám phá construct levels (L1–L3) qua các ví dụ cho thấy increasing abstraction và best practices.
* Có được practical understanding về **ClickOps vs IaC** và cách drift detection giúp duy trì infrastructure consistency.

### Tận dụng công cụ hiện đại (Tóm tắt)

* Học cách **AWS CDK** cho phép infrastructure provisioning thông qua programming languages.
* Khám phá cách **Amplify** sử dụng CloudFormation behind the scenes cho backend deployments.
* Review Docker workflows và làm việc với **ECR, ECS, EKS**, và **App Runner** cho container deployments.
* Hiểu cách managed services tự động hóa scaling, security, và deployment pipelines.

### Networking và Discussions (Tóm tắt)

* Thảo luận real-world IaC adoption challenges với AWS Community Builders và peers.
* Có được insights về việc chọn giữa **ECS và EKS** tùy thuộc vào operational needs và complexity.
* Chia sẻ experiences về migrating từ manual operations sang automated DevOps practices.

### Bài học rút ra (Tóm tắt)

* IaC tăng **automation, consistency, và reproducibility**, giảm human error.
* CloudFormation cung cấp precise control, trong khi CDK boosts developer productivity với higher-level constructs.
* Chọn đúng container service (ECS, EKS, App Runner) ảnh hưởng đến scalability và operational effort.
* Effective DevOps kết hợp IaC, CI/CD, containers, và automation để xây dựng resilient cloud architectures.

### Hình ảnh sự kiện

<img src="/images/event2.1.jpg" alt="Event 2.1" width="450">

<img src="/images/event2.jpg" alt="Event 2" width="450">
