---
title: "Cài đặt Môi trường"
date: 2025-12-01
weight: 2
chapter: false
pre: " <b> 5.02. </b> "
---
#### Tổng quan

Trong bước này, bạn sẽ cài đặt tất cả các công cụ cần thiết để phát triển và deploy ứng dụng EveryoneCook.

#### Công cụ cần thiết

**1. Node.js 20.x**

```bash
# Tải từ https://nodejs.org/
# Hoặc sử dụng nvm (khuyến nghị)
nvm install 20
nvm use 20

# Xác minh cài đặt
node --version  # Phải là v20.x
npm --version
```

**2. AWS CLI v2**

```bash
# Windows: Tải từ https://aws.amazon.com/cli/
# macOS: brew install awscli
# Linux: 
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

# Xác minh cài đặt
aws --version
```

**3. AWS CDK CLI**

```bash
# Cài đặt CDK globally
npm install -g aws-cdk

# Xác minh cài đặt
cdk --version
```

**4. Git**

```bash
# Tải từ https://git-scm.com/
# Hoặc sử dụng package manager

# Xác minh cài đặt
git --version
```

**5. Code Editor**

Khuyến nghị: Visual Studio Code với các extensions:

- AWS Toolkit
- GitLab Workflow
- ESLint
- Prettier

#### Thiết lập tài khoản AWS

**1. Tạo tài khoản AWS**

Nếu bạn chưa có tài khoản AWS:

1. Truy cập https://aws.amazon.com/
2. Click "Create an AWS Account"
3. Làm theo quy trình đăng ký
4. Thêm phương thức thanh toán

**2. Tạo IAM User**

Vì lý do bảo mật, không sử dụng root account

1. Vào IAM Console → Users → Create user
2. Tạo user và lưu credentials

**3. Cấu hình AWS CLI**

```bash
# Cấu hình AWS credentials
aws configure

# Nhập:
# AWS Access Key ID: [Access Key của bạn]
# AWS Secret Access Key: [Secret Key của bạn]
# Default region name: us-east-1
# Default output format: json
```

**4. Xác minh quyền truy cập AWS**

```bash
# Kiểm tra AWS credentials
aws sts get-caller-identity

# Sẽ trả về account ID và user ARN của bạn
```

#### Thiết lập Domain (Tùy chọn)

Nếu bạn muốn sử dụng custom domain:

**1. Đăng ký Domain**

Mua tên miền trên hpanel.hostinger
Route 53 tạo dns record đến hostinger
Trong workshop này, chúng ta sử dụng: `everyonecook.cloud`

**2. Ghi chú Domain Registrar**

Bạn sẽ cần quyền truy cập vào domain registrar để cập nhật nameservers sau này.

#### Thiết lập GitLab

**Tạo GitLab Repo**
*Screenshot: GitLab hiển thị personal access token đã tạo*

**3. Cấu hình Git**

```bash
# Đặt tên và email của bạn
git config --global user.name "Tên của bạn"
git config --global user.email "email.cua.ban@example.com"

# Xác minh cấu hình
git config --list
```

#### Thiết lập Project

**1. Clone hoặc Tạo Project**

Tùy chọn A: Clone project có sẵn

```bash
git clone https://gitlab.com/your-username/everyonecook.git
cd everyonecook
```

Tùy chọn B: Tạo project mới

```bash
mkdir everyonecook
cd everyonecook
git init
```

**2. Cài đặt Dependencies**

```bash
# Cài đặt tất cả dependencies
npm install

# Lệnh này cài đặt:
# - Infrastructure dependencies (CDK)
# - Backend dependencies (Lambda modules)
# - Shared dependencies
```

**3. Copy Environment Variables**

```bash
# Copy file env mẫu
cp .env.example .env

# Chỉnh sửa .env với giá trị của bạn
# Các biến quan trọng:
# - AWS_REGION=us-east-1
# - AWS_ACCOUNT_ID=your-account-id
# - DOMAIN_NAME=everyonecook.cloud
# - GITLAB_TOKEN=your-gitlab-token
```

#### Xác minh

Kiểm tra mọi thứ đã được cài đặt đúng:

```bash
# Kiểm tra Node.js
node --version  # v20.x

# Kiểm tra npm
npm --version   # 10.x

# Kiểm tra AWS CLI
aws --version   # aws-cli/2.x

# Kiểm tra CDK
cdk --version   # 2.x

# Kiểm tra Git
git --version   # 2.x

# Kiểm tra AWS credentials
aws sts get-caller-identity

# Kiểm tra project dependencies
npm list --depth=0
```

#### Xử lý sự cố

**Vấn đề: Node.js version không khớp**

```bash
# Sử dụng nvm để chuyển version
nvm install 20
nvm use 20
```

**Vấn đề: AWS CLI không tìm thấy**

- Khởi động lại terminal sau khi cài đặt
- Kiểm tra biến môi trường PATH

**Vấn đề: CDK command không tìm thấy**

```bash
# Cài đặt lại CDK globally
npm uninstall -g aws-cdk
npm install -g aws-cdk
```

**Vấn đề: AWS credentials không hợp lệ**

```bash
# Cấu hình lại AWS CLI
aws configure
# Nhập credentials đúng
```

#### Ước tính chi phí

**Môi trường Development:**

* AWS Free Tier bao gồm hầu hết các dịch vụ
* Chi phí ước tính: $20-55/tháng
* Chi phí chính: DynamoDB, S3, CloudFront, Lambda

**Mẹo giảm chi phí:**

* Sử dụng pay-per-request cho DynamoDB
* Bật S3 Intelligent-Tiering
* Tắt OpenSearch trong dev (chỉ bật khi cần)
* Xóa tài nguyên khi không sử dụng

#### Bước tiếp theo

Sau khi môi trường đã được thiết lập, tiếp tục đến [CDK Bootstrap](../5.03-cdk-bootstrap/) để chuẩn bị tài khoản AWS cho CDK deployments.
