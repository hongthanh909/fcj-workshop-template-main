---
title: "Triển khai Backend Services"
date: 2025-12-01
weight: 7
chapter: false
pre: " <b> 5.07. </b> "
---

#### Tổng quan

Trong bước này, bạn sẽ build và deploy Lambda function code cho ứng dụng EveryoneCook.

#### Build Lambda Code

```powershell
# Di chuyển đến project root
cd D:\Project_AWS\everyonecook

# Build tất cả Lambda modules
npm run build:all

# Hoặc build từng module
cd services/api-router && npm run build && cd ../..
cd services/auth-user && npm run build && cd ../..
cd services/social && npm run build && cd ../..
cd services/recipe-ai && npm run build && cd ../..
cd services/admin && npm run build && cd ../..
cd services/upload && npm run build && cd ../..
```

#### Deploy Lambda Code

```powershell
# Deploy tất cả Lambda functions
.\deploy\deploy-backend.ps1 -Environment dev

# Hoặc deploy từng function
aws lambda update-function-code `
  --function-name everyonecook-dev-api-router `
  --zip-file fileb://services/api-router/lambda.zip
```

#### Xác minh Deployment

```powershell
# Kiểm tra Lambda functions
aws lambda list-functions `
  --query 'Functions[?contains(FunctionName, `everyonecook-dev`)].FunctionName'

# Test invoke
aws lambda invoke `
  --function-name everyonecook-dev-api-router `
  --payload '{"httpMethod":"GET","path":"/health"}' `
  response.json

cat response.json
```

---

### Bước tiếp theo

➡️ **[5.08 - Kiểm thử Endpoints](../5.08-test-endpoints/)** - Xác minh API functionality
