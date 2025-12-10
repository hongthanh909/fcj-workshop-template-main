---
title: "Dọn dẹp tài nguyên"
date: 2025-12-01
weight: 11
chapter: false
pre: " <b> 5.11. </b> "
---

#### Tổng quan

Trong bước này, bạn sẽ xóa tất cả tài nguyên AWS đã tạo để tránh phát sinh chi phí không cần thiết.

#### Thứ tự xóa

**QUAN TRỌNG:** Xóa stacks theo thứ tự ngược lại với thứ tự triển khai:

```
1. Observability Stack
2. Backend Stack
3. Auth Stack
4. Core Stack
5. Certificate Stack
6. DNS Stack
```

#### Xóa Stacks

```powershell
# Di chuyển đến thư mục infrastructure
cd D:\Project_AWS\everyonecook\infrastructure

# Xóa Observability Stack
npx cdk destroy EveryoneCook-dev-Observability --context environment=dev --force

# Xóa Backend Stack
npx cdk destroy EveryoneCook-dev-Backend --context environment=dev --force

# Xóa Auth Stack
npx cdk destroy EveryoneCook-dev-Auth --context environment=dev --force

# Xóa Core Stack
npx cdk destroy EveryoneCook-dev-Core --context environment=dev --force

# Xóa Certificate Stack (us-east-1)
npx cdk destroy EveryoneCook-dev-Certificate --context environment=dev --force

# Xóa DNS Stack
npx cdk destroy EveryoneCook-dev-DNS --context environment=dev --force
```

#### Xóa S3 Buckets (nếu không tự động xóa)

```powershell
# Xóa nội dung bucket trước
aws s3 rm s3://everyonecook-content-dev --recursive
aws s3 rm s3://everyonecook-cdn-logs-dev --recursive

# Xóa buckets
aws s3 rb s3://everyonecook-content-dev
aws s3 rb s3://everyonecook-cdn-logs-dev
```

#### Xóa CloudWatch Log Groups

```powershell
# Liệt kê log groups
aws logs describe-log-groups --log-group-name-prefix /aws/lambda/everyonecook-dev

# Xóa từng log group
aws logs delete-log-group --log-group-name /aws/lambda/everyonecook-dev-api-router
# ... repeat for other log groups
```

#### Xóa Amplify App (nếu có)

1. Vào **AWS Amplify** Console
2. Chọn app `everyonecook`
3. Click **Actions** > **Delete app**
4. Xác nhận xóa

#### Xác minh xóa hoàn tất

```powershell
# Kiểm tra không còn stacks
aws cloudformation list-stacks `
  --stack-status-filter CREATE_COMPLETE UPDATE_COMPLETE `
  --query 'StackSummaries[?contains(StackName, `EveryoneCook-dev`)].StackName'

# Kết quả mong đợi: empty list
```

#### Khôi phục Nameservers (tùy chọn)

Nếu bạn muốn sử dụng domain cho mục đích khác:

1. Đăng nhập vào domain registrar (Hostinger)
2. Khôi phục nameservers về mặc định của registrar

---

### Hoàn thành Workshop

🎉 **Chúc mừng!** Bạn đã hoàn thành workshop EveryoneCook!

#### Những gì bạn đã học

- ✅ Infrastructure as Code với AWS CDK (TypeScript)
- ✅ Kiến trúc Serverless với API Gateway và Lambda
- ✅ DynamoDB Single Table Design
- ✅ CloudFront CDN với Origin Access Control
- ✅ Cognito authentication với Lambda triggers
- ✅ SQS-based async processing
- ✅ CloudWatch monitoring và alerting
- ✅ Cost optimization strategies

#### Tài nguyên tham khảo

- [AWS CDK Documentation](https://docs.aws.amazon.com/cdk/)
- [DynamoDB Best Practices](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/best-practices.html)
- [CloudFront Developer Guide](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/)
- [Cognito Developer Guide](https://docs.aws.amazon.com/cognito/latest/developerguide/)
