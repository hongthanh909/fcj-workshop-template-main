---
title: "Workshop"
date: 2025-12-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---
# Xây dựng EveryoneCook: Workshop Hạ tầng AWS Full-Stack

#### Tổng quan

Trong workshop toàn diện này, bạn sẽ học cách xây dựng hạ tầng ứng dụng nấu ăn xã hội full-stack sẵn sàng cho production trên AWS sử dụng Infrastructure as Code (IaC) với AWS CDK. Nền tảng **EveryoneCook** minh họa các mẫu kiến trúc cloud hiện đại, bao gồm serverless computing, phân phối nội dung, tính năng AI, tìm kiếm nâng cao với OpenSearch, và giám sát toàn diện.

Bạn sẽ triển khai bảy CDK stack liên kết với nhau để tạo ra một ứng dụng có khả năng mở rộng, bảo mật và tối ưu chi phí:

+ **DNS Stack** - Route 53 hosted zone để quản lý domain
+ **Certificate Stack** - Chứng chỉ ACM cho CloudFront và API Gateway (us-east-1)
+ **CoreStack** - DynamoDB Single Table, S3 buckets, CloudFront CDN, mã hóa KMS, và OpenSearch
+ **AuthStack** - Cognito User Pool với Lambda triggers và tích hợp SES email
+ **BackendStack** - API Gateway, Lambda functions (6 modules), SQS queues, và bảo vệ WAF
+ **FrontendStack** - AWS Amplify hosting cho ứng dụng Next.js 15 (tùy chọn)
+ **ObservabilityStack** - CloudWatch dashboards, alarms, và X-Ray distributed tracing

#### Nội dung

1. [Tổng quan Workshop](5.01-workshop-overview)
2. [Cài đặt Môi trường](5.02-setup-environment/)
3. [CDK Bootstrap](5.03-cdk-bootstrap/)
4. [Cấu hình Infrastructure Stacks](5.04-configure-stacks/)
5. [Triển khai Infrastructure](5.05-deploy-infrastructure/)
6. [Cấu hình API &amp; Lambda](5.06-configure-api-lambda/)
7. [Triển khai Backend Services](5.07-deploy-backend/)
8. [Kiểm thử Endpoints End-to-End](5.08-test-endpoints/)
9. [Push lên GitLab](5.09-push-gitlab/)
10. [Triển khai lên Amplify](5.10-deploy-amplify/)
11. [Dọn dẹp tài nguyên](5.11-cleanup/)
