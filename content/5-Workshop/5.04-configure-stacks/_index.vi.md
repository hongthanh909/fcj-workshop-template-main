---
title: "Cấu hình Infrastructure Stacks"
date: 2025-12-01
weight: 4
chapter: false
pre: " <b> 5.04. </b> "
---

# Cấu hình Infrastructure Stacks


## Tổng quan dự án EveryoneCook

### Giới thiệu
EveryoneCook là một nền tảng mạng xã hội để chia sẻ công thức nấu ăn, được xây dựng hoàn toàn trên AWS Cloud. Dự án sử dụng kiến trúc serverless, tận dụng các dịch vụ managed của AWS để đảm bảo khả năng mở rộng, bảo mật và tối ưu chi phí.

### Kiến trúc hệ thống

Dự án được thiết kế với **Kiến trúc 5-Stack** có các lớp rõ ràng:

```
┌─────────────────────────────────────────────────────────────┐
│                    Nền tảng EveryoneCook                     │
├─────────────────────────────────────────────────────────────┤
│  Lớp 1: DNS & Certificate (Nền tảng)                        │
│  ├─ DNS Stack: Route 53 Hosted Zone                         │
│  └─ Certificate Stack: ACM Certificates (us-east-1)         │
├─────────────────────────────────────────────────────────────┤
│  Lớp 2: Data & Storage (Hạ tầng Core)                       │
│  └─ Core Stack: DynamoDB, S3, CloudFront, KMS               │
├─────────────────────────────────────────────────────────────┤
│  Lớp 3: Authentication & Security                           │
│  └─ Auth Stack: Cognito, Lambda Triggers, SES               │
├─────────────────────────────────────────────────────────────┤
│  Lớp 4: Application & Business Logic                        │
│  └─ Backend Stack: API Gateway, Lambda, SQS, WAF            │
├─────────────────────────────────────────────────────────────┤
│  Lớp 5: Monitoring & Observability                          │
│  └─ Observability Stack: CloudWatch, Alarms, Dashboards     │
└─────────────────────────────────────────────────────────────┘
```

### Công nghệ sử dụng

#### Infrastructure as Code
- **AWS CDK (TypeScript)**: Quản lý hạ tầng bằng code
- **CloudFormation**: Engine template cơ bản cho CDK
- **Git**: Version control cho infrastructure code

#### Backend Services
- **API Gateway REST API**: RESTful API endpoint với custom domain
- **Lambda Functions**: Serverless compute cho business logic (6 functions chính + 1 worker)
- **Lambda Layers**: Shared dependencies layer (giảm deployment size 90%)
- **DynamoDB**: NoSQL database với Single Table Design + 5 GSI indexes
- **S3**: Object storage cho user content (avatars, posts, recipes, backgrounds)
- **SQS**: Message queue cho xử lý bất đồng bộ (4 queues + 4 DLQs)

#### Security & Authentication
- **Cognito User Pool**: Xác thực & quản lý người dùng
- **Cognito User Pool Client**: Cấu hình xác thực frontend
- **WAF (Web Application Firewall)**: Bảo vệ API Gateway với rate limiting
- **KMS (Key Management Service)**: 2 customer-managed keys (DynamoDB, S3)
- **IAM Roles & Policies**: Kiểm soát truy cập chi tiết cho tất cả dịch vụ

#### Content Delivery & Networking
- **CloudFront**: CDN với Origin Access Control (OAC) + Shield Standard
- **Route 53**: Quản lý DNS với Hosted Zone
- **ACM (Certificate Manager)**: Chứng chỉ SSL/TLS (2 certificates)

#### Monitoring & Operations
- **CloudWatch Logs**: Logging tập trung cho Lambda functions
- **CloudWatch Metrics**: Theo dõi metrics hiệu suất
- **CloudWatch Dashboards**: 4 dashboards (Core, Auth, Backend, Overview)
- **CloudWatch Alarms**: Giám sát real-time (10+ alarms) + Composite Alarm
- **SNS (Simple Notification Service)**: Email notifications cho alarms

### Cấu trúc thư mục dự án

```
everyonecook/
├── infrastructure/                    # AWS CDK Infrastructure
│   ├── bin/
│   │   └── app.ts                    # CDK app entry point - tạo tất cả stacks
│   ├── lib/
│   │   ├── base-stack.ts             # Base class cho tất cả stacks
│   │   ├── stacks/                   # Định nghĩa Stack
│   │   │   ├── dns-stack.ts          # Route 53 Hosted Zone
│   │   │   ├── certificate-stack.ts  # ACM Certificates
│   │   │   ├── core-stack.ts         # DynamoDB, S3, CloudFront
│   │   │   ├── auth-stack.ts         # Cognito, Lambda triggers
│   │   │   ├── backend-stack.ts      # API Gateway, Lambda, SQS
│   │   │   └── observability-stack.ts # CloudWatch, Alarms
│   │   └── constructs/               # CDK constructs tái sử dụng
│   │       └── shared-layer.ts       # Lambda Layer với dependencies
│   ├── config/
│   │   └── environment.ts            # Cấu hình môi trường (dev/staging/prod)
│   ├── cdk.json                      # Cấu hình CDK
│   ├── package.json                  # Node.js dependencies
│   └── tsconfig.json                 # Cấu hình TypeScript
├── services/                         # Lambda function source code
│   ├── api-router/                   # API request routing
│   ├── auth-user/                    # Authentication endpoints
│   ├── social/                       # Social features (posts, comments)
│   ├── recipe-ai/                    # Recipe & AI endpoints
│   ├── admin/                        # Admin management
│   └── upload/                       # File upload handler
├── shared/                           # Shared code & utilities
│   ├── utils/                        # Common utilities
│   ├── models/                       # Data models
│   └── middleware/                   # Lambda middleware
├── frontend/                         # Next.js frontend (deploy riêng)
│   └── ...
└── layers/                           # Lambda layers
  └── shared-dependencies/          # Common npm packages
```

## Tổng quan kiến trúc Stack

### 1. DNS Stack (Phase 1)
**Mục đích**: Tạo nền tảng cho quản lý DNS

**Tài nguyên chính**:
- Route 53 Hosted Zone cho domain `everyonecook.cloud`
- Export nameservers để cấu hình tại domain registrar

**Thứ tự triển khai**: Stack này phải được triển khai đầu tiên

**Chi phí ước tính**: ~$0.50/tháng

> **Lưu ý**: Sau khi triển khai stack này, cập nhật nameservers tại domain registrar của bạn (ví dụ: Hostinger) để trỏ đến Route 53.

---

### 2. Certificate Stack (Phase 1.5)
**Mục đích**: Tạo chứng chỉ SSL/TLS cho CloudFront và API Gateway

**Tài nguyên chính**:
- ACM Certificate cho CloudFront: `cdn.everyonecook.cloud`
- ACM Wildcard Certificate cho API Gateway: `*.everyonecook.cloud`
- DNS validation records trong Route 53

**Region đặc biệt**: **PHẢI triển khai tại us-east-1** (yêu cầu của CloudFront)

**Dependencies**: DNS Stack (cần Hosted Zone cho DNS validation)

**Chi phí ước tính**: Miễn phí (ACM certificates miễn phí)

> **Quan trọng**: CloudFront chỉ chấp nhận certificates từ region us-east-1.

---

### 3. Core Stack (Phase 2)
**Mục đích**: Tạo lớp dữ liệu và hạ tầng storage

**Tài nguyên chính**:
- **DynamoDB Table**: Single Table Design với 5 GSI indexes
- **S3 Buckets** (2 buckets): Content và CDN Logs
- **CloudFront Distribution**: CDN với OAC
- **KMS Keys** (2 keys): DynamoDB và S3

**Dependencies**: Certificate Stack (cần certificate cho CloudFront)

**Chi phí ước tính**: ~$8-15/tháng

---

### 4. Auth Stack (Phase 3)
**Mục đích**: Xác thực và quản lý người dùng

**Tài nguyên chính**:
- **Cognito User Pool**: Với password policy
- **Cognito User Pool Client**
- **Lambda Triggers** (5 functions)
- **IAM Roles** cho Lambda functions

**Dependencies**: Core Stack (Lambda triggers cần truy cập DynamoDB)

**Chi phí ước tính**: ~$0-2/tháng (Cognito free tier: 50,000 MAU)

---

### 5. Backend Stack (Phase 4)
**Mục đích**: Lớp ứng dụng và business logic

**Tài nguyên chính**:
- **API Gateway REST API** với custom domain
- **Lambda Functions** (6 modules + 1 worker)
- **SQS Queues** (4 queues + 4 DLQs)
- **WAF Web ACL** cho API Gateway
- **Lambda Layer** (shared dependencies)

**Dependencies**: Auth Stack (cần Cognito User Pool)

**Chi phí ước tính**: ~$10-25/tháng

---

### 6. Observability Stack (Phase 7)
**Mục đích**: Giám sát, logging, và alerting

**Tài nguyên chính**:
- **CloudWatch Dashboards** (4 dashboards)
- **CloudWatch Alarms** (15+ alarms)
- **Composite Alarm** cho overall health
- **SNS Topic** cho notifications

**Dependencies**: Tất cả stacks khác (giám sát toàn bộ hạ tầng)

**Chi phí ước tính**: ~$3-8/tháng

---

### Luồng Dependencies của Stack

```
DNS Stack (Route 53)
  ↓
Certificate Stack (ACM tại us-east-1)
  ↓
Core Stack (DynamoDB, S3, CloudFront, KMS)
  ↓
Auth Stack (Cognito, Lambda Triggers)
  ↓
Backend Stack (API Gateway, Lambda, SQS, WAF)
  ↓
Observability Stack (CloudWatch, Alarms)
```

### Tổng chi phí ước tính

**Môi trường Development**:
| Dịch vụ | Chi phí/tháng | Ghi chú |
|---------|--------------|---------|
| Route 53 Hosted Zone | $0.50 | 1 hosted zone + DNS queries |
| ACM Certificates | $0 | Miễn phí cho public certificates |
| DynamoDB (Pay-per-request) | $3-5 | Phụ thuộc vào usage |
| S3 (Intelligent-Tiering) | $1-3 | 2 buckets, auto-tiering |
| CloudFront | $2-5 | CDN distribution + data transfer |
| Lambda | $0-3 | 7 functions + 1 worker |
| API Gateway | $0-3 | REST API |
| Cognito (Free tier) | $0 | Đến 50,000 MAU |
| SQS | $0-1 | 4 queues + 4 DLQs |
| WAF (API Gateway only) | $5-8 | Web ACL + rules |
| CloudWatch | $1-3 | Logs, Dashboards, Alarms |
| KMS | $2 | 2 customer-managed keys |
| **TỔNG** | **$15-35/tháng** | Development với traffic thấp |

---

## Hướng dẫn cấu hình

### Bước 1: Chuẩn bị thư mục Infrastructure

**1. Di chuyển đến thư mục infrastructure**

```powershell
cd D:\Project_AWS\everyonecook\infrastructure
```

**2. Cài đặt dependencies**

```powershell
npm install
```

### Bước 2: Cấu hình Environment Settings

**1. Xem lại cấu hình môi trường**

Mở file `config/environment.ts` để xem cấu hình:

```powershell
code config\environment.ts
```

**2. Xác minh AWS Account ID**

```powershell
# Kiểm tra AWS Account ID hiện tại
aws sts get-caller-identity
```

### Bước 3: Xem lại cấu trúc CDK App

**1. Xem file CDK app chính**

```powershell
code bin\app.ts
```

### Bước 4: Validate Configuration

**1. Compile TypeScript**

```powershell
npm run build
```

**2. Liệt kê tất cả CDK stacks**

```powershell
npx cdk list --context environment=dev
```

**3. Synthesize CloudFormation templates**

```powershell
npx cdk synth --context environment=dev
```

---

## Checklist cấu hình

Trước khi triển khai, kiểm tra các mục sau:

- [ ] **Cấu hình môi trường**
  - [ ] AWS Account ID đã được xác minh và cập nhật
  - [ ] Region được đặt đúng (ap-southeast-1 cho dev)
  - [ ] Domain names được cấu hình đúng
  
- [ ] **Dependencies**
  - [ ] Node.js và npm đã được cài đặt
  - [ ] AWS CLI đã được cấu hình với credentials
  - [ ] AWS CDK CLI đã được cài đặt globally
  - [ ] npm dependencies đã được cài đặt (`npm install`)

- [ ] **Validation**
  - [ ] TypeScript compilation thành công (`npm run build`)
  - [ ] CDK list hiển thị tất cả 6 stacks
  - [ ] CDK synth tạo CloudFormation templates thành công

---

## Bước tiếp theo

Sau khi hoàn thành cấu hình và validation, tiếp tục đến:

**[5.05 Triển khai Infrastructure](../5.05-deploy-infrastructure/)** - Triển khai tất cả stacks lên AWS

Trong bước tiếp theo, bạn sẽ:
1. Triển khai DNS Stack và cấu hình nameservers
2. Triển khai Certificate Stack và validate certificates
3. Triển khai Core Stack (DynamoDB, S3, CloudFront)
4. Triển khai Auth Stack (Cognito, Lambda Triggers)
5. Triển khai Backend Stack (API Gateway, Lambda Functions)
6. Triển khai Observability Stack (CloudWatch Dashboards)
