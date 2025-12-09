---
title: "Deploy Backend Services"
date: 2025-12-01
weight: 7
chapter: false
pre: " <b> 5.07. </b> "
---

#### Overview

After deploying infrastructure with CDK, you need to deploy Lambda code to AWS. The EveryoneCook project provides an automated deployment script to build, package, and deploy all Lambda functions.

#### Backend Architecture

**Lambda Modules (7 functions):**
- `api-router` - API Gateway routing với JWT validation
- `auth-user` - Authentication & User Management
- `social` - Posts, Comments, Reactions, Friends, Notifications
- `recipe-ai` - Recipes & AI Features (Bedrock)
- `ai-worker` - Async AI processing worker
- `admin` - Admin Dashboard & Content Moderation
- `upload` - File Upload với S3 Presigned URLs

**Lambda Layer:**
- `shared-dependencies` - Shared npm packages (aws-sdk, uuid, etc.)

**Cognito Triggers (Auth Stack):**
- Pre-Signup, Post-Confirmation, Post-Authentication, Pre-Authentication, Custom Message

#### Deployment Process

```
1. Build Lambda Layer (shared dependencies)
2. Build TypeScript modules → JavaScript
3. Validate dist folders (check for issues)
4. Prepare deployment packages
5. Deploy via CDK hoặc Direct Lambda Update
6. Verify deployments
7. Check CloudWatch logs
```

---

### Option A: Full Deployment (Recommended - First Time)

Full deployment uses CDK to deploy the entire infrastructure and Lambda code.

#### Step 1: Navigate to Project

```powershell
# Navigate to everyonecook root
cd D:\Project_AWS\everyonecook
```

#### Step 2: Run Full Deployment Script

```powershell
# Full deployment for dev environment
.\deploy\deploy-backend.ps1 -Environment dev

# Or short form (default is dev)
.\deploy\deploy-backend.ps1
```

**Script will perform:**

**STEP 0: Building Lambda Layer**
- Clean previous builds
- Install production dependencies vào `layers/shared-dependencies/nodejs/`
- Layer size: ~15-20 MB

**STEP 1: Building Lambda modules**
- Build auth triggers (Pre-Signup, Post-Confirmation, etc.)
- Build tất cả 7 Lambda modules
- Clean dist and tsconfig.tsbuildinfo to force fresh compile
- Compile TypeScript → JavaScript

**STEP 2: Validating dist folders**
- Check dist folder exists
- Detect node_modules in dist (common issue)
- Check dist size (should be < 10 MB)
- Verify index.js exists
- Auto-repair if issues detected

**STEP 3: Preparing deployment packages**
- Clean previous artifacts
- Copy dist contents → deployment folder
- Add build-info.json (timestamp, git commit)

**STEP 4: Deploy with CDK**
- Synth CloudFormation templates
- Deploy `EveryoneCook-dev-Backend` stack
- Update Lambda functions
- Update Lambda Layer
- Update API Gateway

**Sample output:**

```
========================================
Backend Deployment - dev
With Lambda Layers
========================================

STEP 0: Building Lambda Layer...
  Installing layer dependencies...
  Layer built: 18.45 MB

STEP 1: Building Lambda modules...
  Building auth triggers... OK
  Building api-router... OK
  Building auth-module... OK
  Building social-module... OK
  Building ai-module... OK
  Building admin-module... OK
  Building upload-module... OK

STEP 2: Validating dist folders...
  api-router... OK (125 files, 1.2 MB)
  auth-module... OK (156 files, 1.5 MB)
  social-module... OK (189 files, 1.8 MB)
  ai-module... OK (142 files, 1.3 MB)
  admin-module... OK (98 files, 0.9 MB)
  upload-module... OK (45 files, 0.4 MB)

STEP 3: Preparing deployment packages...
  Preparing api-router... Done
  Preparing auth-module... Done
  Preparing social-module... Done
  Preparing ai-module... Done
  Preparing admin-module... Done
  Preparing upload-module... Done

STEP 4: Deploying with CDK...
  Synthesizing CloudFormation templates...
  
EveryoneCook-dev-Backend
  Deploying...
  ✅ Lambda Layer updated
  ✅ API Router function updated
  ✅ Auth User function updated
  ✅ Social function updated
  ✅ Recipe AI function updated
  ✅ AI Worker updated
  ✅ Admin function updated
  ✅ Upload function updated
  
Outputs:
  ApiUrl = https://xinq7xh300.execute-api.ap-southeast-1.amazonaws.com/v1/
  ApiCustomDomain = https://api-dev.everyonecook.cloud

========================================
FULL DEPLOYMENT SUCCESSFUL!
========================================
```

#### Step 3: Verify Deployment

```powershell
# List all Lambda functions
aws lambda list-functions `
  --query 'Functions[?contains(FunctionName, `everyonecook-dev`)].{Name:FunctionName,Runtime:Runtime,Updated:LastModified}' `
  --output table

# Expected output:
# --------------------------------------------------------
# |                   ListFunctions                      |
# +------------------------------------------------------+
# |  Name                           | Runtime | Updated  |
# +------------------------------------------------------+
# |  everyonecook-dev-api-router    | nodejs20.x | ...   |
# |  everyonecook-dev-auth-user     | nodejs20.x | ...   |
# |  everyonecook-dev-social        | nodejs20.x | ...   |
# |  everyonecook-dev-recipe-ai     | nodejs20.x | ...   |
# |  everyonecook-dev-ai-worker     | nodejs20.x | ...   |
# |  everyonecook-dev-admin         | nodejs20.x | ...   |
# |  everyonecook-dev-upload        | nodejs20.x | ...   |
# +------------------------------------------------------+
```

---

### Option B: Fast Deployment (Lambda Code Only)

If you only change Lambda code (no infrastructure changes), use fast deployment.

#### Step 1: Lambda Only Deploy

```powershell
# Fast deploy - only update Lambda code
.\deploy\deploy-backend.ps1 -Environment dev -LambdaOnly

# Or use dedicated script
.\deploy\force-update-lambdas.ps1 -Environment dev
```

**Faster deployment process:**

```
STEP 0: Build Layer (hoặc skip)
STEP 1: Build modules
STEP 2: Validate dist
STEP 3: Prepare packages
STEP 4: Upload to Lambda (Fast Deploy)
  - Create ZIP files
  - Upload directly via aws lambda update-function-code
  - Update Layer ARN if changed
  - Skip CDK (faster)
```

**Time:**
- Full Deploy (CDK): ~5-8 minutes
- Lambda Only: ~2-3 minutes

#### Step 2: Skip Options (Advanced)

```powershell
# Skip layer build (use existing layer)
.\deploy\deploy-backend.ps1 -Environment dev -SkipLayer

# Skip module build (use existing dist)
.\deploy\deploy-backend.ps1 -Environment dev -SkipBuild

# Combine options
.\deploy\deploy-backend.ps1 -Environment dev -LambdaOnly -SkipLayer
```

---

### Step 4: Verify Lambda Functions

#### 1. Check Function Configuration

```powershell
# Get API Router details
aws lambda get-function `
  --function-name everyonecook-dev-api-router `
  --query 'Configuration.{Runtime:Runtime,Handler:Handler,Timeout:Timeout,Memory:MemorySize,Layer:Layers[0].Arn}'

# Expected output:
# {
#   "Runtime": "nodejs20.x",
#   "Handler": "services/api-router/dist/handlers/index.handler",
#   "Timeout": 30,
#   "Memory": 512,
#   "Layer": "arn:aws:lambda:ap-southeast-1:...:layer:everyonecook-shared-deps-dev:X"
# }
```

#### 2. Verify Code SHA256 (Code đã update)

```powershell
# Check code hash
aws lambda get-function `
  --function-name everyonecook-dev-api-router `
  --query 'Configuration.CodeSha256'

# SHA256 will change each time code is updated
```

#### 3. Check Environment Variables

```powershell
# Get environment variables
aws lambda get-function-configuration `
  --function-name everyonecook-dev-auth-user `
  --query 'Environment.Variables'

# Expected:
# {
#   "TABLE_NAME": "EveryoneCook-dev",
#   "REGION": "ap-southeast-1",
#   "USER_POOL_ID": "ap-southeast-1_PKoL34PF0",
#   ...
# }
```

---

### Step 5: Test Lambda Functions

#### 1. Test API Router (Health Check)

```powershell
# Invoke API Router với test event
aws lambda invoke `
  --function-name everyonecook-dev-api-router `
  --payload '{"httpMethod":"GET","path":"/health","headers":{}}' `
  response.json

# Check response
Get-Content response.json | ConvertFrom-Json

# Expected:
# {
#   "statusCode": 200,
#   "body": "{\"status\":\"healthy\",\"timestamp\":\"...\"}"
# }
```

#### 2. Warm Up Functions (Avoid Cold Start)

```powershell
# Warm up all functions
$functions = @(
  "everyonecook-dev-api-router",
  "everyonecook-dev-auth-user",
  "everyonecook-dev-social",
  "everyonecook-dev-recipe-ai",
  "everyonecook-dev-admin",
  "everyonecook-dev-upload"
)

foreach ($func in $functions) {
  Write-Host "Warming up $func..." -ForegroundColor Cyan
  aws lambda invoke `
    --function-name $func `
    --payload '{"warmup":true}' `
    out.json | Out-Null
}

Write-Host "All functions warmed up!" -ForegroundColor Green
```

---

### Step 6: Check CloudWatch Logs

#### 1. Tail Logs Real-time

```powershell
# Tail API Router logs
aws logs tail /aws/lambda/everyonecook-dev-api-router --follow

# View last 10 minutes
aws logs tail /aws/lambda/everyonecook-dev-api-router --since 10m
```

#### 2. Search for Errors

```powershell
# Search for errors in last hour
$oneHourAgo = [DateTimeOffset]::Now.AddHours(-1).ToUnixTimeMilliseconds()

aws logs filter-log-events `
  --log-group-name /aws/lambda/everyonecook-dev-api-router `
  --start-time $oneHourAgo `
  --filter-pattern "ERROR"
```

#### 3. Check All Lambda Logs

```powershell
# List all log groups
aws logs describe-log-groups `
  --log-group-name-prefix /aws/lambda/everyonecook-dev `
  --query 'logGroups[].logGroupName'
```

---

### Step 7: Verify API Gateway Integration

#### 1. Get API Endpoint

```powershell
# Get API URL from CloudFormation
$apiUrl = aws cloudformation describe-stacks `
  --stack-name EveryoneCook-dev-Backend `
  --query 'Stacks[0].Outputs[?OutputKey==`ApiCustomDomain`].OutputValue' `
  --output text

Write-Host "API Endpoint: $apiUrl"
# Output: https://api-dev.everyonecook.cloud
```

#### 2. Test Health Endpoint

```powershell
# Test via API Gateway (real endpoint)
$response = Invoke-RestMethod -Uri "$apiUrl/health" -Method Get
$response | ConvertTo-Json

# Expected:
# {
#   "status": "healthy",
#   "timestamp": "2025-12-09T...",
#   "service": "EveryoneCook API",
#   "environment": "dev"
# }
```

#### 3. Verify API Gateway Deployment

```powershell
# Get API ID
$apiId = aws cloudformation describe-stacks `
  --stack-name EveryoneCook-dev-Backend `
  --query 'Stacks[0].Outputs[?OutputKey==`ApiId`].OutputValue' `
  --output text

# List deployments
aws apigateway get-deployments --rest-api-id $apiId

# Get latest deployment
aws apigateway get-deployment `
  --rest-api-id $apiId `
  --deployment-id (aws apigateway get-deployments `
    --rest-api-id $apiId `
    --query 'items[0].id' `
    --output text)
```

---

### Step 8: Verify Cognito Triggers

Cognito triggers were deployed with Auth Stack. Verify:

```powershell
# List Cognito User Pool triggers
$userPoolId = aws cloudformation describe-stacks `
  --stack-name EveryoneCook-dev-Auth `
  --query 'Stacks[0].Outputs[?OutputKey==`UserPoolId`].OutputValue' `
  --output text

aws cognito-idp describe-user-pool `
  --user-pool-id $userPoolId `
  --query 'UserPool.LambdaConfig'

# Expected:
# {
#   "PreSignUp": "arn:aws:lambda:...:function:EveryoneCook-dev-PreSignup",
#   "PostConfirmation": "arn:aws:lambda:...:function:EveryoneCook-dev-PostConfirmation",
#   "PostAuthentication": "arn:aws:lambda:...:function:EveryoneCook-dev-PostAuthentication",
#   "PreAuthentication": "arn:aws:lambda:...:function:EveryoneCook-dev-PreAuthentication",
#   "CustomMessage": "arn:aws:lambda:...:function:EveryoneCook-dev-CustomMessage"
# }
```

---

### Step 9: Verify SQS Workers

#### 1. Check AI Worker

```powershell
# Get AI Worker function
aws lambda get-function --function-name everyonecook-dev-ai-worker

# Check event source mapping (SQS trigger)
aws lambda list-event-source-mappings `
  --function-name everyonecook-dev-ai-worker

# Expected: Event source từ AI Queue
```

#### 2. Test Worker với SQS Message

```powershell
# Get AI Queue URL
$queueUrl = aws sqs list-queues `
  --queue-name-prefix everyonecook-dev-ai-queue `
  --query 'QueueUrls[0]' `
  --output text

# Send test message
aws sqs send-message `
  --queue-url $queueUrl `
  --message-body '{"type":"test","data":"worker-verification"}'

# Check worker logs
aws logs tail /aws/lambda/everyonecook-dev-ai-worker --follow
```

---

### Deployment Checklist

Use this checklist to verify deployment:

#### Build & Package
- [ ] Lambda Layer built successfully (~15-20 MB)
- [ ] All 7 modules compiled (TypeScript → JavaScript)
- [ ] Dist folders validated (no node_modules, < 10 MB)
- [ ] Deployment packages created with build-info.json

#### Lambda Functions
- [ ] All 7 Lambda functions deployed
- [ ] Runtime: nodejs20.x
- [ ] Handler paths correct
- [ ] Timeout: 30 seconds
- [ ] Memory: 512 MB (auth, social), 1024 MB (ai)

- [ ] Lambda Layer attached to all functions
- [ ] Environment variables configured correctly
- [ ] Code SHA256 changed (code updated)

#### Cognito Triggers
- [ ] Pre-Signup trigger deployed
- [ ] Post-Confirmation trigger deployed
- [ ] Post-Authentication trigger deployed
- [ ] Pre-Authentication trigger deployed
- [ ] Custom Message trigger deployed

#### API Gateway
- [ ] API Gateway deployed
- [ ] Custom domain configured: `api-dev.everyonecook.cloud`
- [ ] Health endpoint responding: `GET /health`
- [ ] CORS configured correctly

#### Workers & Queues
- [ ] AI Worker deployed
- [ ] Event source mapping configured (SQS → Lambda)
- [ ] SQS queues created (ai-queue, image-queue, analytics-queue, notification-queue)
- [ ] Worker can process test messages

#### Verification
- [ ] All functions invoked successfully
- [ ] No errors in CloudWatch logs
- [ ] API Gateway returns 200 for health check
- [ ] Functions warmed up (no cold start)

---

### Troubleshooting

#### Issue 1: Build Failed - TypeScript Compilation Error

```powershell
# Check TypeScript errors
cd services/auth-module
npx tsc --noEmit

# Fix errors và rebuild
npm run build
```

#### Issue 2: Dist Folder Too Large

```powershell
# Check for node_modules in dist
Get-ChildItem -Path services/*/dist/node_modules -Recurse

# Script will auto-remove, or manual:
Remove-Item services/auth-module/dist/node_modules -Recurse -Force

# Rebuild
cd services/auth-module
npm run build
```

#### Issue 3: Lambda Update Failed

```powershell
# Check function state
aws lambda get-function `
  --function-name everyonecook-dev-auth-user `
  --query 'Configuration.{State:State,StateReason:StateReason}'

# If state is "Failed", check logs
aws logs tail /aws/lambda/everyonecook-dev-auth-user --since 10m
```

#### Issue 4: Function Timeout

```powershell
# Increase timeout (via CDK or AWS CLI)
aws lambda update-function-configuration `
  --function-name everyonecook-dev-auth-user `
  --timeout 60

# Or update in backend-stack.ts and redeploy
```

#### Issue 5: Out of Memory

```powershell
# Increase memory
aws lambda update-function-configuration `
  --function-name everyonecook-dev-recipe-ai `
  --memory-size 1024

# AI functions need 1024 MB for Bedrock calls
```

#### Issue 6: Layer Not Attached

```powershell
# Get layer ARN
$layerArn = aws lambda list-layer-versions `
  --layer-name everyonecook-shared-deps-dev `
  --query 'LayerVersions[0].LayerVersionArn' `
  --output text

# Attach layer manually
aws lambda update-function-configuration `
  --function-name everyonecook-dev-auth-user `
  --layers $layerArn
```

#### Issue 7: Environment Variables Missing

```powershell
# Check current env vars
aws lambda get-function-configuration `
  --function-name everyonecook-dev-auth-user `
  --query 'Environment.Variables'

# Update manually (or via CDK)
aws lambda update-function-configuration `
  --function-name everyonecook-dev-auth-user `
  --environment "Variables={TABLE_NAME=EveryoneCook-dev,REGION=ap-southeast-1}"
```

---

### Performance Monitoring

#### 1. Lambda Metrics (CloudWatch)

```powershell
# Get invocation count (last 1 hour)
$startTime = (Get-Date).AddHours(-1).ToString("yyyy-MM-ddTHH:mm:ss")
$endTime = (Get-Date).ToString("yyyy-MM-ddTHH:mm:ss")

aws cloudwatch get-metric-statistics `
  --namespace AWS/Lambda `
  --metric-name Invocations `
  --dimensions Name=FunctionName,Value=everyonecook-dev-api-router `
  --start-time $startTime `
  --end-time $endTime `
  --period 300 `
  --statistics Sum
```

#### 2. Error Rate

```powershell
# Get error count
aws cloudwatch get-metric-statistics `
  --namespace AWS/Lambda `
  --metric-name Errors `
  --dimensions Name=FunctionName,Value=everyonecook-dev-api-router `
  --start-time $startTime `
  --end-time $endTime `
  --period 300 `
  --statistics Sum
```

#### 3. Duration (Average & P99)

```powershell
# Get average duration
aws cloudwatch get-metric-statistics `
  --namespace AWS/Lambda `
  --metric-name Duration `
  --dimensions Name=FunctionName,Value=everyonecook-dev-api-router `
  --start-time $startTime `
  --end-time $endTime `
  --period 300 `
  --statistics Average,Maximum
```

#### 4. Concurrent Executions

```powershell
# Check concurrent executions
aws cloudwatch get-metric-statistics `
  --namespace AWS/Lambda `
  --metric-name ConcurrentExecutions `
  --dimensions Name=FunctionName,Value=everyonecook-dev-api-router `
  --start-time $startTime `
  --end-time $endTime `
  --period 300 `
  --statistics Maximum
```

---

### Best Practices

#### 1. Deployment Strategy

**Development:**
```powershell
# Fast iteration - Lambda Only
.\deploy\deploy-backend.ps1 -LambdaOnly
```

**Staging/Production:**
```powershell
# Full deployment with CDK
.\deploy\deploy-backend.ps1 -Environment prod

# Verify thoroughly before proceeding
```

#### 2. Rollback Strategy

```powershell
# Get previous version
aws lambda list-versions-by-function `
  --function-name everyonecook-dev-auth-user

# Rollback to version X
aws lambda update-alias `
  --function-name everyonecook-dev-auth-user `
  --name LIVE `
  --function-version X
```

#### 3. Blue-Green Deployment

```powershell
# Deploy to new version
.\deploy\deploy-backend.ps1 -Environment dev

# Test new version
# If OK, traffic shift complete
# If issues, rollback alias
```

#### 4. Monitoring Alerts

```powershell
# Create CloudWatch alarm for errors
aws cloudwatch put-metric-alarm `
  --alarm-name everyonecook-dev-api-router-errors `
  --alarm-description "Alert on Lambda errors" `
  --metric-name Errors `
  --namespace AWS/Lambda `
  --statistic Sum `
  --period 300 `
  --evaluation-periods 1 `
  --threshold 10 `
  --comparison-operator GreaterThanThreshold `
  --dimensions Name=FunctionName,Value=everyonecook-dev-api-router
```

---

### Performance Benchmarks

**Expected Deployment Times:**

| Operation | Time | Notes |
|-----------|------|-------|
| Build Layer | 30-60s | npm install production deps |
| Build Modules | 20-40s | TypeScript compilation (7 modules) |
| Validate & Package | 10-20s | ZIP creation |
| CDK Deploy | 3-5 min | CloudFormation update |
| Lambda Only | 1-2 min | Direct function update |

**Expected Lambda Performance:**

| Function | Cold Start | Warm | Memory | Timeout |
|----------|-----------|------|--------|---------|
| api-router | 800-1200ms | 50-100ms | 512 MB | 30s |
| auth-user | 600-900ms | 80-150ms | 512 MB | 30s |
| social | 700-1000ms | 100-200ms | 512 MB | 30s |
| recipe-ai | 1500-2500ms | 200-500ms | 1024 MB | 60s |
| ai-worker | 2000-3000ms | 300-800ms | 1024 MB | 300s |
| admin | 600-800ms | 100-150ms | 512 MB | 30s |
| upload | 400-600ms | 50-80ms | 256 MB | 15s |

---

### Advanced: CI/CD Integration

To automate deployment in GitLab CI/CD:

```yaml
# .gitlab-ci.yml
deploy-backend:
  stage: deploy
  script:
    - cd everyonecook
    - .\deploy\deploy-backend.ps1 -Environment $CI_ENVIRONMENT_NAME -LambdaOnly
  only:
    - main
  environment:
    name: production
```

---

### Summary

In this lab, you have:

✅ **Deploy Lambda Functions**: 7 modules + 5 Cognito triggers  
✅ **Deploy Lambda Layer**: Shared dependencies  
✅ **Configure API Gateway**: Custom domain integration  
✅ **Setup SQS Workers**: Async processing  
✅ **Verify Deployment**: Health checks, logs, metrics  
✅ **Performance Tuning**: Warm up functions, optimize cold start  

**Key Achievements:**
- Automated deployment với PowerShell script
- Full CDK deployment hoặc fast Lambda-only update
- Comprehensive validation và error handling
- CloudWatch monitoring và alerting
- Production-ready Lambda configuration

---

### Next Steps

Backend deployed successfully! Next steps:

1. ✅ **Test Endpoints**: Verify tất cả API endpoints → [5.08 - Test Endpoints](../5.08-test-endpoints/)
2. 📝 **Version Control**: Push code to GitLab → [5.09 - Push to GitLab](../5.09-push-gitlab/)
3. 🚀 **Deploy Frontend**: Next.js application
4. 🔍 **Monitor**: CloudWatch dashboards và alerts

Proceed to: [5.08 - Test Endpoints End-to-End](../5.08-test-endpoints/)

```bash
# Test get posts
aws lambda invoke \
  --function-name EveryoneCook-dev-SocialModule \
  --payload '{
    "httpMethod":"GET",
    "path":"/social/posts",
    "headers":{"Authorization":"Bearer test-token"},
    "requestContext":{"authorizer":{"claims":{"sub":"test-user-id","username":"testuser"}}}
  }' \
  response.json

cat response.json
```

#### Step 5: Check CloudWatch Logs

**1. View Recent Logs**

```bash
# Tail Auth Module logs
aws logs tail /aws/lambda/EveryoneCook-dev-AuthModule --follow

# Or view last 10 minutes
aws logs tail /aws/lambda/EveryoneCook-dev-AuthModule --since 10m
```

**2. Search for Errors**

```bash
# Search for errors in last hour
aws logs filter-log-events \
  --log-group-name /aws/lambda/EveryoneCook-dev-AuthModule \
  --start-time $(date -d '1 hour ago' +%s)000 \
  --filter-pattern "ERROR"
```

**3. Check Lambda Insights**

```bash
# Get Lambda metrics
aws cloudwatch get-metric-statistics \
  --namespace AWS/Lambda \
  --metric-name Invocations \
  --dimensions Name=FunctionName,Value=EveryoneCook-dev-AuthModule \
  --start-time $(date -u -d '1 hour ago' +%Y-%m-%dT%H:%M:%S) \
  --end-time $(date -u +%Y-%m-%dT%H:%M:%S) \
  --period 300 \
  --statistics Sum
```

![CloudWatch Logs](/images/5-Workshop/5.7-deploy-backend/cloudwatch-logs.png)
*Screenshot: CloudWatch Logs showing Lambda execution*

#### Step 6: Deploy Lambda Triggers

**Lambda triggers đã được deploy với Auth Stack, verify:**

```bash
# Check Post-Confirmation trigger
aws lambda get-function \
  --function-name EveryoneCook-dev-PostConfirmationTrigger

# Test trigger (will be invoked automatically on user confirmation)
```

#### Step 7: Deploy Search Sync Worker

**1. Deploy Worker**

```bash
# Worker được deploy với Backend Stack
# Verify deployment
aws lambda get-function \
  --function-name EveryoneCook-dev-SearchSyncWorker
```

**2. Test Worker with SQS**

```bash
# Get SearchIndex queue URL
QUEUE_URL=$(aws sqs list-queues \
  --queue-name-prefix EveryoneCook-dev-SearchIndexQueue \
  | jq -r '.QueueUrls[0]')

# Send test message
aws sqs send-message \
  --queue-url $QUEUE_URL \
  --message-body '{
    "eventName": "INSERT",
    "tableName": "recipes",
    "keys": {"PK": "USER#testuser", "SK": "RECIPE#test-123"},
    "newImage": {"title": "Test Recipe", "cuisine": "Vietnamese"}
  }'

# Check worker logs
aws logs tail /aws/lambda/EveryoneCook-dev-SearchSyncWorker --follow
```

#### Step 8: Update API Gateway

**API Gateway tự động update khi deploy Backend Stack, verify:**

```bash
# Get API Gateway deployment
API_ID=$(aws cloudformation describe-stacks \
  --stack-name EveryoneCook-dev-Backend \
  --query 'Stacks[0].Outputs[?OutputKey==`ApiId`].OutputValue' \
  --output text)

# List deployments
aws apigateway get-deployments --rest-api-id $API_ID

# Get latest deployment
aws apigateway get-deployment \
  --rest-api-id $API_ID \
  --deployment-id $(aws apigateway get-deployments \
    --rest-api-id $API_ID \
    --query 'items[0].id' \
    --output text)
```

#### Step 9: Warm Up Lambda Functions

**Tránh cold start cho requests đầu tiên:**

```bash
# Invoke all functions once
for func in APIRouter AuthModule SocialModule RecipeAIModule AdminModule UploadModule SearchSyncWorker; do
  echo "Warming up $func..."
  aws lambda invoke \
    --function-name EveryoneCook-dev-$func \
    --payload '{"warmup":true}' \
    /dev/null
done
```

#### Step 10: Verify End-to-End

**1. Test Health Endpoint**

```bash
# Test via API Gateway
curl https://api.everyonecook.cloud/health

# Should return: {"status":"healthy","timestamp":"..."}
```

**2. Test with Postman**

Import Postman collection:
```json
{
  "info": {
    "name": "EveryoneCook API",
    "schema": "https://schema.getpostman.com/json/collection/v2.1.0/collection.json"
  },
  "item": [
    {
      "name": "Health Check",
      "request": {
        "method": "GET",
        "url": "https://api.everyonecook.cloud/health"
      }
    }
  ]
}
```

#### Deployment Checklist

- [ ] Preparation script completed successfully
- [ ] All Lambda functions updated
- [ ] Function status: Active
- [ ] Test invocations successful
- [ ] CloudWatch logs showing executions
- [ ] No errors in logs
- [ ] Lambda triggers working
- [ ] Search Sync Worker processing messages
- [ ] API Gateway updated
- [ ] Health endpoint responding
- [ ] Functions warmed up

#### Troubleshooting

**Issue: Lambda update fails**

```bash
# Check function state
aws lambda get-function \
  --function-name EveryoneCook-dev-AuthModule \
  --query 'Configuration.State'

# If state is "Failed", check StateReasonCode
aws lambda get-function \
  --function-name EveryoneCook-dev-AuthModule \
  --query 'Configuration.StateReasonCode'
```

**Issue: Function timeout**

```bash
# Increase timeout
aws lambda update-function-configuration \
  --function-name EveryoneCook-dev-AuthModule \
  --timeout 30
```

**Issue: Out of memory**

```bash
# Increase memory
aws lambda update-function-configuration \
  --function-name EveryoneCook-dev-AuthModule \
  --memory-size 512
```

**Issue: Environment variables missing**

```bash
# Update environment variables
aws lambda update-function-configuration \
  --function-name EveryoneCook-dev-AuthModule \
  --environment Variables={TABLE_NAME=EveryoneCook-dev,REGION=us-east-1}
```

#### Performance Monitoring

**Check Lambda metrics:**

```bash
# Invocations
aws cloudwatch get-metric-statistics \
  --namespace AWS/Lambda \
  --metric-name Invocations \
  --dimensions Name=FunctionName,Value=EveryoneCook-dev-AuthModule \
  --start-time $(date -u -d '1 hour ago' +%Y-%m-%dT%H:%M:%S) \
  --end-time $(date -u +%Y-%m-%dT%H:%M:%S) \
  --period 300 \
  --statistics Sum

# Errors
aws cloudwatch get-metric-statistics \
  --namespace AWS/Lambda \
  --metric-name Errors \
  --dimensions Name=FunctionName,Value=EveryoneCook-dev-AuthModule \
  --start-time $(date -u -d '1 hour ago' +%Y-%m-%dT%H:%M:%S) \
  --end-time $(date -u +%Y-%m-%dT%H:%M:%S) \
  --period 300 \
  --statistics Sum

# Duration
aws cloudwatch get-metric-statistics \
  --namespace AWS/Lambda \
  --metric-name Duration \
  --dimensions Name=FunctionName,Value=EveryoneCook-dev-AuthModule \
  --start-time $(date -u -d '1 hour ago' +%Y-%m-%dT%H:%M:%S) \
  --end-time $(date -u +%Y-%m-%dT%H:%M:%S) \
  --period 300 \
  --statistics Average,Maximum
```

#### Next Steps

Once backend is deployed and verified, proceed to [Test Endpoints End-to-End](../5.8-test-endpoints/) to test the complete application flow.
