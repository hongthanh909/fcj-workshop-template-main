---
title: "Tổng quan Workshop"
date: 2025-12-01
weight: 1
chapter: false
pre: " <b> 5.01. </b> "
---
#### Kiến trúc EveryoneCook

**EveryoneCook** là một nền tảng nấu ăn xã hội hiện đại được xây dựng hoàn toàn trên các công nghệ serverless của AWS. Kiến trúc tuân theo các best practices về khả năng mở rộng, bảo mật và tối ưu chi phí, với trọng tâm hỗ trợ nguyên liệu Việt Nam và gợi ý công thức nấu ăn bằng AI.

#### Các thành phần chính

+ **Frontend**: Ứng dụng Next.js 15 với Flowbite React components được host trên AWS Amplify
+ **Backend**: Serverless API sử dụng API Gateway với API Router pattern và 6 Lambda modules
+ **Database**: DynamoDB Single Table Design với username-based PK và 5 GSI indexes
+ **Storage**: S3 buckets với Intelligent-Tiering và CloudFront CDN với OAC
+ **Authentication**: Cognito User Pool với Lambda triggers cho custom flows
+ **Tích hợp AI**: Amazon Bedrock (Claude 3 Haiku - anthropic.claude-3-haiku-20240307-v1:0) cho dịch nguyên liệu, tra cứu dinh dưỡng, và tạo công thức
+ **Bảo mật**: WAF cho API Gateway, Shield Standard cho CloudFront, mã hóa KMS
+ **Giám sát**: CloudWatch dashboards, alarms, và X-Ray distributed tracing

#### Sơ đồ kiến trúc

![Kiến trúc EveryoneCook](/images/Fsocialarchitecture.png)

#### Quy trình Workshop

Workshop này tuân theo quy trình phát triển ứng dụng thực tế:

1. **Cài đặt Môi trường** - Cài đặt công cụ (Node.js, AWS CLI, CDK CLI)
2. **CDK Bootstrap** - Chuẩn bị tài khoản AWS cho CDK deployments
3. **Cấu hình Stacks** - Thiết lập cấu hình infrastructure (DNS, Certificate, Core, Auth, Backend, Observability)
4. **Triển khai Infrastructure** - Deploy tất cả CDK stacks lên AWS
5. **Cấu hình API & Lambda** - Thiết lập API Gateway routes và Lambda functions
6. **Triển khai Backend** - Deploy API và Lambda code
7. **Kiểm thử Endpoints** - Xác minh tất cả endpoints hoạt động end-to-end
8. **Push lên GitLab** - Version control và thiết lập CI/CD
9. **Triển khai lên Amplify** - Deploy frontend lên AWS Amplify
10. **Giám sát & Bảo trì** - Sử dụng CloudWatch và X-Ray để giám sát

#### Bạn sẽ học được gì

+ Infrastructure as Code với AWS CDK (TypeScript)
+ Kiến trúc Serverless với API Router pattern
+ DynamoDB Single Table Design với PROVISIONED billing mode
+ CloudFront CDN với Origin Access Control (OAC) - PriceClass 200
+ Xác thực Cognito với Lambda triggers
+ Tổ chức Lambda function theo module (6 modules: API Router, Auth/User, Social, Recipe/AI, Admin, Upload)
+ Xử lý bất đồng bộ dựa trên SQS với 4 queues và workers
+ Tích hợp Bedrock AI (Claude 3 Haiku) với chiến lược dictionary-first caching
+ Bảo mật WAF cho API Gateway (REGIONAL scope)
+ Giám sát CloudWatch với structured logging (X-Ray disabled để tối ưu chi phí)
+ Chiến lược tối ưu chi phí cho các kịch bản traffic thấp

#### Ước tính chi phí thực tế (Kịch bản Traffic thấp)

**Kịch bản 1: 100-500 Người dùng hoạt động/Ngày (2 giờ sử dụng)**

- **Monthly Active Users**: ~3,000-15,000
- **Daily Requests**: ~5,000-25,000 API calls
- **Chi phí hàng tháng ước tính**: **$12-18/tháng** ($0.40-0.60/ngày)

**Kịch bản 2: 1,000 Người dùng hoạt động/Ngày (2 giờ sử dụng)**

- **Monthly Active Users**: ~30,000
- **Daily Requests**: ~50,000 API calls
- **Chi phí hàng tháng ước tính**: **$25-35/tháng** ($0.83-1.17/ngày)

**Các yếu tố chi phí chính**:

+ **DynamoDB**: PROVISIONED mode (2 RCU/WCU) với auto-scaling → $1.25/tháng base + $0.50-3/tháng scaling
+ **Lambda**: 6 functions (512MB-1GB memory) → $2-8/tháng (1M requests đầu tiên miễn phí)
+ **CloudFront**: PriceClass 200 (Asia/US/Europe) → $1-5/tháng (1TB đầu tiên miễn phí, sau đó $0.085/GB)
+ **API Gateway**: Regional endpoint → $1-3/tháng (1M đầu tiên miễn phí, sau đó $3.50/triệu)
+ **Cognito**: Standard security (không có Advanced Security Mode) → $0-1.50/tháng (50K MAU đầu tiên miễn phí)
+ **S3**: Intelligent-Tiering → $0.50-2/tháng (50GB đầu tiên miễn phí)
+ **Bedrock AI**: Claude 3 Haiku ($0.25/1M input tokens) → $2-8/tháng với 99% dictionary cache hit rate
+ **SQS**: 4 queues (AI, Image, Analytics, Notification) → $0-0.40/tháng (1M requests đầu tiên miễn phí)
+ **CloudWatch Logs**: 3-day retention → $0.50-2/tháng
+ **WAF**: Chỉ API Gateway (không có CloudFront WAF) → $5/tháng base + $0.60/triệu requests

**Tối ưu chi phí đã áp dụng**:

+ X-Ray tracing disabled (tiết kiệm $5-10/tháng)
+ CloudFront WAF removed, sử dụng Shield Standard (tiết kiệm $6/tháng)
+ DynamoDB PROVISIONED mode với baseline thấp (2 RCU/WCU) thay vì ON_DEMAND
+ CloudWatch log retention giảm xuống 3 ngày cho dev (tiết kiệm $2-5/tháng)
+ API Gateway caching disabled cho dev (tiết kiệm $14.60/tháng)
+ Chiến lược Bedrock dictionary-first (99% cache hit rate, giảm chi phí AI 70%)
+ Lambda Shared Dependencies Layer (giảm deployment size 90%, cold starts nhanh hơn)

#### Tính năng chính

+ **Hỗ trợ tiếng Việt**: Vietnamese analyzer cho tìm kiếm nguyên liệu
+ **AI-Powered**: Bedrock Claude 3 Haiku cho tạo công thức
+ **Dictionary-First Translation**: Mục tiêu 80% coverage với intelligent caching
+ **Nền tảng xã hội**: Posts, comments, reactions, friends, notifications
+ **Field-Level Privacy**: Kiểm soát chi tiết về hiển thị profile
+ **Content Moderation**: Quy trình kiểm duyệt tự động và thủ công
+ **Tìm kiếm nâng cao**: Full-text search với Vietnamese normalization
+ **Tối ưu chi phí**: Intelligent-Tiering, caching, và tối ưu tài nguyên
