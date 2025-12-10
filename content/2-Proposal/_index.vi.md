---
title: "Bản đề xuất"
date: 2025-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---
# AWS First Cloud AI Journey – Kế hoạch dự án

**Hello World – FPT University – EveryoneCook**

**Ngày:** 30/11/2025

📥 **[Tải Proposal đầy đủ (DOCX)](/Proposal%20Template.docx)**

---

# MỤC LỤC

**1. [BỐI CẢNH VÀ ĐỘNG LỰC](#1-bối-cảnh-và-động-lực)**

&nbsp;&nbsp;&nbsp;&nbsp;1.1 [Tóm tắt điều hành](#11-tóm-tắt-điều-hành)

&nbsp;&nbsp;&nbsp;&nbsp;1.2 [Tiêu chí thành công dự án](#12-tiêu-chí-thành-công-dự-án)

&nbsp;&nbsp;&nbsp;&nbsp;1.3 [Giả định](#13-giả-định)

**2. [KIẾN TRÚC GIẢI PHÁP / SƠ ĐỒ KIẾN TRÚC](#2-kiến-trúc-giải-pháp--sơ-đồ-kiến-trúc)**

&nbsp;&nbsp;&nbsp;&nbsp;2.1 [Sơ đồ kiến trúc kỹ thuật](#21-sơ-đồ-kiến-trúc-kỹ-thuật)

&nbsp;&nbsp;&nbsp;&nbsp;2.2 [Kế hoạch kỹ thuật](#22-kế-hoạch-kỹ-thuật)

&nbsp;&nbsp;&nbsp;&nbsp;2.3 [Kế hoạch dự án](#23-kế-hoạch-dự-án)

&nbsp;&nbsp;&nbsp;&nbsp;2.4 [Các cân nhắc bảo mật](#24-các-cân-nhắc-bảo-mật)

**3. [HOẠT ĐỘNG VÀ SẢN PHẨM BÀN GIAO](#3-hoạt-động-và-sản-phẩm-bàn-giao)**

&nbsp;&nbsp;&nbsp;&nbsp;3.1 [Hoạt động và sản phẩm bàn giao](#31-hoạt-động-và-sản-phẩm-bàn-giao)

&nbsp;&nbsp;&nbsp;&nbsp;3.2 [Ngoài phạm vi](#32-ngoài-phạm-vi)

&nbsp;&nbsp;&nbsp;&nbsp;3.3 [Lộ trình đưa vào Production](#33-lộ-trình-đưa-vào-production)

**4. [DỰ KIẾN CHI PHÍ AWS THEO DỊCH VỤ](#4-dự-kiến-chi-phí-aws-theo-dịch-vụ)**

**5. [NHÓM](#5-nhóm)**

**6. [NGUỒN LỰC & ƯỚC TÍNH CHI PHÍ](#6-nguồn-lực--ước-tính-chi-phí)**

**7. [CHẤP THUẬN](#7-chấp-thuận)**

# 1. BỐI CẢNH VÀ ĐỘNG LỰC

## 1.1 Tóm tắt điều hành

**Bối cảnh khách hàng**

Khách hàng là một startup tập trung vào việc xây dựng một nền tảng mạng xã hội hiện đại nơi người dùng có thể chia sẻ công thức nấu ăn, tải lên ảnh món ăn, trao đổi kinh nghiệm ẩm thực, và khám phá các món ăn được AI gợi ý. Tổ chức nhằm mục đích cung cấp một nền tảng tương tác cao có khả năng phục vụ lượng người dùng lớn và ngày càng tăng.

**Mục tiêu kinh doanh và kỹ thuật -- động lực chuyển sang AWS cloud**

- Cho phép phát triển và triển khai nhanh chóng sử dụng các dịch vụ AWS managed
- Đảm bảo khả năng mở rộng cao khi lượng người dùng và media storage tăng
- Cung cấp môi trường đáng tin cậy, độ trễ thấp cho AI computation và content delivery
- Giảm chi phí hạ tầng ban đầu và chuyển sang mô hình pay-as-you-go
- Cải thiện bảo mật dữ liệu, backup, và compliance thông qua các khả năng AWS-native

**Use cases**

- Người dùng tải lên công thức, ảnh, và video nấu ăn lên nền tảng
- Hệ thống gợi ý món ăn sử dụng AI dựa trên nguyên liệu có sẵn do người dùng cung cấp
- Người dùng tương tác xã hội thông qua like, comment, share, và follow
- AI xử lý text và images để tạo gợi ý công thức
- Admins quản lý nội dung, giám sát hoạt động nền tảng, và theo dõi performance analytics

Để đáp ứng mục tiêu của khách hàng về việc xây dựng một nền tảng social cooking có khả năng mở rộng với AI-powered recipe recommendations, partner sẽ cung cấp triển khai cloud end-to-end hoàn chỉnh trên AWS. Các dịch vụ được cung cấp bao gồm:

- **Cloud Architecture Design:** Định nghĩa kiến trúc serverless an toàn, có khả năng mở rộng cao sử dụng AWS best practices (Route 53, API Gateway, Lambda, DynamoDB, S3, CloudFront, Cognito)
- **AI Integration:** Triển khai AWS Bedrock (Claude 3.5 Sonnet) cho intelligent recipe suggestions, image analysis, và natural language processing features
- **Infrastructure Deployment:** Xây dựng và triển khai tất cả backend, frontend, authentication, và data layers sử dụng Infrastructure as Code (IaC) với fully automated CI/CD pipelines
- **Security & Compliance:** Cấu hình IAM roles, encryption (KMS), WAF, logging, monitoring, và compliance guardrails để đảm bảo platform security
- **Observability Setup:** Kích hoạt CloudWatch dashboards, alarms, X-Ray tracing, và log centralization cho real-time monitoring và performance insights
- **DevOps & Automation:** Triển khai automated build/deploy workflows qua GitLab + Amplify, operational pipelines, và auto-scaling configurations
- **Performance Optimization:** Cấu hình CDN caching, DynamoDB capacity scaling, search indexing, và asynchronous SQS-based background processing
- **Knowledge Transfer & Documentation:** Cung cấp technical documentation, best practices, architectural guides, và handover training cho engineering team của khách hàng

## 1.2 Tiêu chí thành công dự án

- System availability ≥ 99.9% uptime trên tất cả production services (API Gateway, Lambda, DynamoDB, CloudFront).
- Page load time < 2.5 giây cho main user interface được deliver qua CloudFront và Amplify.
- API response time < 300 ms cho 90% tất cả user-facing API requests trong điều kiện traffic bình thường.
- AI processing latency < 5 giây cho recipe suggestions được tạo bởi AWS Bedrock.
- User authentication success rate ≥ 98% với Cognito xử lý registration, login, và email verification.
- Zero critical security vulnerabilities sau security review và WAF rules deployment.
- Data durability 99.999999999% (11 nines) được đảm bảo thông qua S3 object storage và DynamoDB.
- Scalability để hỗ trợ 10,000+ concurrent users mà không giảm performance nhờ serverless infrastructure.
- Operational cost control trong target budget: monthly AWS usage không vượt quá $200 cho production.
- Image upload & processing success rate ≥ 99%, được hỗ trợ bởi S3, Lambda Workers, và SQS.
- Search performance dưới 1 giây (nếu OpenSearch được kích hoạt) cho recipe/content search queries.
- Monitoring coverage 100% critical services sử dụng CloudWatch dashboards, alarms, và X-Ray tracing.
- CI/CD deployment time < 5 phút qua GitHub → Amplify và IaC automation.
- Zero data loss events, được đảm bảo bởi DynamoDB PITR và S3 versioning.

## 1.3 Giả định

- Khách hàng sẽ cung cấp full access đến domain registrar (Hostinger) để cấu hình DNS delegation đến Route 53.
- Khách hàng sẽ cung cấp valid AWS account access với Administrator privileges cho deployment và configuration.
- Tất cả required AWS services (Amplify, API Gateway, Lambda, DynamoDB, CloudFront, Cognito, Bedrock, SES) đều available và supported trong chosen region.
- SES sẽ được successfully moved out of sandbox và approved cho production email sending.
- Third-party integrations (GitHub cho CI/CD, external email clients, image upload sources) sẽ remain available và stable.
- Development team sẽ maintain source code quality và follow architectural guidelines được cung cấp bởi partner.
- Khách hàng sẽ cung cấp timely feedback và approvals trong các giai đoạn design, testing, và deployment.

**Dependencies**

- Reliable internet connectivity được yêu cầu cho tất cả users truy cập web application và APIs.
- Hệ thống phụ thuộc vào AWS Bedrock (Claude 3.5 Sonnet) cho AI recipe generation và có thể experience performance fluctuations nếu model bị rate-limited.
- Image upload và processing workflows phụ thuộc vào S3, Lambda, và SQS processing reliability.
- Nếu OpenSearch được kích hoạt, search features rely on availability của OpenSearch domain.
- GitHub Actions và Amplify phụ thuộc vào GitHub service availability.

**Constraints**

- Dự án sẽ được fully deployed trong một single AWS region, có thể impact latency cho users ngoài region.
- Giải pháp được thiết kế sử dụng serverless patterns; custom EC2-based workloads nằm ngoài project scope.
- SES domain reputation có thể affect email deliverability trong những tuần đầu.
- OpenSearch được deployed như single-node cluster cho cost efficiency, nghĩa là không có high availability cho search indexing.
- Hệ thống phải stay within customer's cost target (< $200/month), limiting việc sử dụng large compute resources.

**Risks**

- SES production approval có thể bị delayed, impacting user onboarding emails và notifications.
- Nếu traffic grows unexpectedly, DynamoDB provisioned capacity có thể throttle mà không có timely scaling adjustments.
- AI model cost hoặc latency changes bởi AWS có thể impact application performance hoặc cost control.
- Misconfigured CloudFront caching có thể lead to higher latency hoặc increased data transfer cost.
- Bất kỳ incorrect IAM configuration nào có thể lead to security risks hoặc service disruption.
- Customer team turnover hoặc lack of DevOps skills có thể slow down future maintenance hoặc deployments.

# 2. KIẾN TRÚC GIẢI PHÁP / SƠ ĐỒ KIẾN TRÚC

## 2.1 Sơ đồ kiến trúc kỹ thuật

- Kiến trúc giải pháp được đề xuất cho nền tảng social network nấu ăn AI-powered được thiết kế sử dụng fully serverless và scalable AWS cloud-native stack. Kiến trúc đảm bảo high availability, security, và seamless integration giữa web frontend, backend APIs, authentication, data storage, và AI recommendation services.
- Dưới đây là mô tả các key components và cách data flows qua hệ thống:

![F Social Architecture](/images/Fsocialarchitecture.png)

**1. Network & Edge Layer**

**Amazon Route 53 (6–7)**

- Cung cấp DNS routing cho custom domain được sử dụng bởi nền tảng.
- Incoming HTTPS requests từ users được resolved và forwarded đến CloudFront.

**Amazon CloudFront (9)**

- Hoạt động như global CDN distributing frontend content với low latency trong khi caching static files.

**AWS WAF (8)**

- Bảo vệ application khỏi common web exploits như SQL injection, XSS, và bot attacks.

**2. Frontend Hosting & Deployment**

**AWS Amplify Hosting (4)**

- Hosts và deploys Next.js frontend application.
- Integrated với **GitLab CI/CD (3)** cho automated deployments từ development workflow.

**3. Application Layer**

**Amazon Cognito (10)**

- Xử lý user authentication và authorization, supporting email/password và social logins.

**Amazon API Gateway (11)**

- Serves như main entry point cho backend APIs, exposing REST endpoints được sử dụng bởi frontend.

**AWS Lambda (12, 15)**

- Chứa backend business logic, bao gồm:
- user management
- post và recipe operations
- ingredient analysis
- connecting đến Bedrock cho AI recommendations

Kiến trúc serverless này đảm bảo automatic scaling và pay-per-use cost efficiency.

**4. AI Recommendation Layer**

**Amazon Bedrock (16–17)**

- Cung cấp generative AI capabilities để suggest recipes dựa trên user-provided ingredients.
- Lambda invokes Bedrock models (e.g., Claude, Titan) để:
- analyze ingredient lists
- generate recipe suggestions
- classify food categories
- optimize cooking steps.

**5. Data Storage Layer**

**Amazon DynamoDB (13)**

Stores structured application data như:

- user profiles
- posts/recipes
- likes & comments
- ingredient metadata.

**Amazon S3 (14)**

Stores unstructured data:

- recipe images
- user-uploaded food photos
- static content.

S3 bucket được integrated với CloudFront qua OAI cho secure access.

**6. Observability & Security Layer**

**Amazon CloudWatch (Logs & Metrics)**
Monitors Lambda performance, API Gateway access logs, và system metrics.

**AWS X-Ray**
Performs distributed tracing cho API calls và debugging.

**IAM**
Defines permission boundaries giữa API, Lambda functions, Bedrock, DynamoDB, và S3.

**Amazon SES**
Sends verification emails, notifications, và password recovery messages.

**Amazon SNS**
Handles system-level alerts và asynchronous messaging.

**7. Deployment & Infrastructure Management**

**AWS CDK (1–2)**
Được developers sử dụng để define và provision toàn bộ infrastructure qua CloudFormation templates.
Ensures consistent, reproducible, version-controlled deployments.

## 2.2 Kế hoạch kỹ thuật

Partner sẽ develop Infrastructure-as-Code (IaC) automation sử dụng AWS CDK (Cloud Development Kit) với TypeScript/Python để provision toàn bộ cloud environment. Approach này ensures quick, consistent, và repeatable deployments trên multiple AWS accounts và environments (dev, staging, production). Tất cả resources như API Gateway, Lambda functions, DynamoDB tables, S3 buckets, Cognito user pools, Bedrock integration policies, và CloudFront distributions sẽ được fully automated qua IaC.

Application build và deployment processes cho frontend (Next.js) sẽ được automated sử dụng AWS Amplify Hosting, integrated với GitLab pipelines. Backend components sẽ được deployed thông qua CDK pipelines để ensure controlled, versioned, và repeatable releases.

## 2.3 Kế hoạch dự án

[Partner] sẽ adopt Agile Scrum framework trên tám 2-week sprints. Stakeholders từ team được yêu cầu participate trong Sprint Reviews và Sprint Retrospectives để ensure alignment và continuous improvement.

Các team responsibilities được đề xuất như sau:

- Product Owner: Define user stories, prioritize backlog, và ensure product meets user needs.
- Scrum Master: Facilitate Scrum ceremonies, remove blockers, và maintain team productivity.
- Development Team: Implement features, conduct unit testing, và collaborate on integration.
- AI/ML Specialist: Develop và fine-tune AI recommendation engine suggest recipes dựa trên user-provided ingredients.
- UI/UX Designer: Design intuitive interfaces và ensure smooth user experience trên cả web và mobile platforms.
- QA/Testers: Validate feature functionality, conduct regression testing, và ensure system reliability.

## 2.4 Các cân nhắc bảo mật

Partner sẽ implement security best practices trên năm categories sau để ensure confidentiality, integrity, và availability của nền tảng:

1. **Access**

- Enable Multi-Factor Authentication (MFA) cho tất cả user và administrative accounts.
- Implement role-based access control (RBAC) để limit permissions dựa trên user roles.
- Enforce strong password policies và periodic password rotation.

2. **Infrastructure**

- Deploy application trên secure, managed cloud services (e.g., AWS) following AWS security best practices.
- Use Virtual Private Cloud (VPC), network segmentation, và security groups để isolate resources.
- Regularly patch operating systems và containerized services để mitigate vulnerabilities.

3. **Data**

- Encrypt tất cả data at rest sử dụng AWS KMS-managed keys và data in transit sử dụng TLS/HTTPS.
- Implement data classification để protect sensitive user information.
- Apply secure data storage và backup procedures để ensure availability và integrity.

4. **Detection**

- Enable AWS CloudTrail và AWS Config để monitor API activity và resource configurations.
- Deploy logging và alerting mechanisms để detect unusual hoặc suspicious activities in real time.
- Conduct periodic vulnerability scanning và penetration testing trên nền tảng.

5. **Incident Management**

- Establish formal incident response plan bao gồm detection, containment, remediation, và communication.
- Maintain audit trails và logs để support forensic investigation nếu security event xảy ra.

# 3. HOẠT ĐỘNG VÀ SẢN PHẨM BÀN GIAO

## 3.2 Ngoài phạm vi

**Real-time Messaging / Chat System**

- Private hoặc group chat
- Real-time messaging infrastructure (WebSocket, SignalR, Firebase Realtime DB, etc.)
- Message history storage & encryption

**Friends / Social Graph Management**

- Friend requests, following/followers
- User-to-user connection graph
- Activity feed, notifications tied to friend actions

**Real-time Voice & Video Calling**

- 1-to-1 hoặc group voice call
- Video call, screen sharing
- WebRTC signaling servers & TURN/STUN infrastructure

**Advanced Social Interaction**

- In-app messaging reactions
- Typing indicators, online/offline status
- Read receipts, presence tracking

## 3.3 Lộ trình đưa vào Production

**1. Project Foundation & Infrastructure**

- Initialize project structure
- Set up core infrastructure baseline
- Configure Route 53 Hosted Zone (DNS Stack)

**2. Cross-Region Certificate (Side Quest)**

- Handle CloudFront requirement cho ACM certificate trong **us-east-1**
- Sync certificate usage với main stack trong **ap-southeast-1**

**3. Core Application Stacks**

- **Core Stack:** Shared resources / environment setup
- **Authentication Stack:** User auth, Cognito, permissions
- **Backend Stack:** API Gateway + Lambda functions

**4. Frontend Deployment**

- Deploy frontend (S3 + CloudFront)
- Bug fixes & QA testing

# 4. DỰ KIẾN CHI PHÍ AWS THEO DỊCH VỤ

Target workload: 100-500 Monthly Active Users (MAU)

Average Lambda duration: 200ms per invocation

DynamoDB peak activity: ~8 hours per month

S3 to CloudFront data transfer là FREE (same region)

Tất cả services leverage AWS Free Tier where applicable (Lambda 1M requests, SQS 1M requests, Cognito 50K MAU, Amplify 1000 build minutes)

API Gateway caching enabled at 0.5GB ($14.60/month) - có thể disabled để reduce costs

CloudFront WAF removed để optimize costs (~$9/month savings), Shield Standard provides DDoS protection

Bedrock uses on-demand pricing với Claude 3 Haiku (lowest cost Anthropic model)

[https://calculator.aws/#/estimate?id=7a8833402a63e273357ddc71071bfc2cdce4be2c](https://calculator.aws/#/estimate?id=7a8833402a63e273357ddc71071bfc2cdce4be2c)

# 5. NHÓM

**Partner Executive Sponsor**

| Tên | Chức danh | Mô tả | Email / Liên hệ |
| --- | --- | --- | --- |
| Nguyen Gia Hung | Director of FCJ Vietnam Training Program | Với tư cách Executive Sponsor, chịu trách nhiệm overall oversight của FCJ internship program | hunggia@amazon.com |

**Project Stakeholders**

| Tên | Chức danh | Stakeholder for | Email / Liên hệ |
| --- | --- | --- | --- |
| Van Hoang Kha | Support Teams | Executive Assistant chịu trách nhiệm overall oversight của FCJ internship program | Khab9thd@gmail.com |

**Partner Project Team**

| Tên | Chức danh | Vai trò | Email / Liên hệ |
| --- | --- | --- | --- |
| Pham Minh Hoang Viet | Leader | Project Manager | vietpmhse181851@gmail.com |
| Nguyen Van Truong | Member | DevOps | truongnvse182034@fpt.edu.vn |
| Huynh Duc Anh | Member | Cloud Engineer | anhhdse183114@fpt.edu.vn |
| Nguyen Thanh Hong | Member | Tester | hongntse183239@fpt.edu.vn |
| Nguyen Quy Duc | Member | Frontend | ducnqse182087@fpt.edu.vn |

# 6. NGUỒN LỰC & ƯỚC TÍNH CHI PHÍ

| **Nguồn lực** | **Trách nhiệm** | **Rate (USD) / Hour** |
| --- | --- | --- |
| Solution Architects | Architecture design, AWS service selection, security review, cost optimization | $150 |
| Engineers | Frontend (Next.js), Backend (Lambda/Node.js), Infrastructure (CDK), Testing | $100 |
| DevOps | CI/CD setup, monitoring, deployment automation | $80 |

**Monthly AWS Infrastructure Cost**

**Dựa trên AWS Pricing Calculator estimate cho 100-500 MAU:**

| Service | Monthly Cost (USD) | Description |
| --- | --- | --- |
| Amazon DynamoDB | $13.06 | Single-table design, 5 GSIs, provisioned capacity |
| Amazon S3 | $0.84 | 2 buckets, Intelligent-Tiering |
| Amazon CloudFront | $1.44 | CDN, Price Class 200 |
| Amazon Cognito | $5.00 | User authentication |
| AWS Lambda | $0.00 | 13 functions (Free Tier) |
| Amazon API Gateway | $20.65 | REST API với 0.5GB cache |
| Amazon SQS | $0.00 | 8 queues (Free Tier) |
| Amazon SES | $0.02 | Transactional emails |
| AWS KMS | $2.00 | 2 customer managed keys |
| AWS WAF | $10.00 | Web ACL, 5 rules |
| Amazon CloudWatch | $21.25 | Metrics, dashboards, alarms, logs |
| Amazon Route 53 | $0.93 | DNS hosted zone |
| AWS Amplify | $4.58 | Frontend hosting (Next.js) |
