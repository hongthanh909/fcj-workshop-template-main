---
title: "Kiểm thử Endpoints End-to-End"
date: 2025-12-01
weight: 8
chapter: false
pre: " <b> 5.08. </b> "
---

#### Tổng quan

Trong bước này, bạn sẽ kiểm thử tất cả API endpoints để xác minh chúng hoạt động đúng.

#### API Base URL

```
https://api-dev.everyonecook.cloud
```

#### Test Health Endpoint

```powershell
# Test health endpoint
$response = Invoke-RestMethod -Uri "https://api-dev.everyonecook.cloud/health" -Method Get
$response | ConvertTo-Json

# Expected:
# {
#   "status": "healthy",
#   "timestamp": "2025-12-09T...",
#   "service": "EveryoneCook API"
# }
```

#### Test Authentication

```powershell
# Register user
$body = @{
  username = "testuser"
  email = "test@example.com"
  password = "TestPassword123!"
  fullName = "Test User"
} | ConvertTo-Json

Invoke-RestMethod -Uri "https://api-dev.everyonecook.cloud/auth/register" `
  -Method Post -Body $body -ContentType "application/json"

# Login
$loginBody = @{
  username = "testuser"
  password = "TestPassword123!"
} | ConvertTo-Json

$loginResponse = Invoke-RestMethod -Uri "https://api-dev.everyonecook.cloud/auth/login" `
  -Method Post -Body $loginBody -ContentType "application/json"

$token = $loginResponse.accessToken
```

#### Test Protected Endpoints

```powershell
# Get user profile (protected)
$headers = @{
  Authorization = "Bearer $token"
}

Invoke-RestMethod -Uri "https://api-dev.everyonecook.cloud/users/testuser" `
  -Method Get -Headers $headers

# Create post (protected)
$postBody = @{
  content = "Hello from EveryoneCook!"
} | ConvertTo-Json

Invoke-RestMethod -Uri "https://api-dev.everyonecook.cloud/posts" `
  -Method Post -Body $postBody -ContentType "application/json" -Headers $headers
```

#### Test CDN

```powershell
# Test CloudFront CDN
curl -I https://cdn-dev.everyonecook.cloud

# Expected: HTTP/2 403 (bucket empty, but CDN working)
```

---

### Bước tiếp theo

➡️ **[5.09 - Push lên GitLab](../5.09-push-gitlab/)** - Version control và CI/CD setup
