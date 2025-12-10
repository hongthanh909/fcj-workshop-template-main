---
title: "Push lên GitLab"
date: 2025-12-01
weight: 9
chapter: false
pre: " <b> 5.09. </b> "
---

#### Tổng quan

Trong bước này, bạn sẽ push code lên GitLab repository và thiết lập CI/CD pipeline.

#### Cấu hình Git Remote

```powershell
# Di chuyển đến project root
cd D:\Project_AWS\everyonecook

# Thêm GitLab remote
git remote add origin https://gitlab.com/your-username/everyonecook.git

# Xác minh remote
git remote -v
```

#### Commit và Push

```powershell
# Stage tất cả changes
git add .

# Commit
git commit -m "Initial commit: EveryoneCook infrastructure and backend"

# Push lên GitLab
git push -u origin main
```

#### GitLab CI/CD

Tạo file `.gitlab-ci.yml` cho CI/CD pipeline:

```yaml
stages:
  - build
  - test
  - deploy

build:
  stage: build
  script:
    - npm install
    - npm run build:all

test:
  stage: test
  script:
    - npm run test

deploy:
  stage: deploy
  script:
    - ./deploy/deploy-backend.ps1 -Environment dev
  only:
    - main
```

---

### Bước tiếp theo

➡️ **[5.10 - Triển khai lên Amplify](../5.10-deploy-amplify/)** - Deploy frontend lên AWS Amplify
