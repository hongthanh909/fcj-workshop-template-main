---
title: "5.4.7 Observability Stack"
weight: 7
---
---
# Observability Stack - Giám sát và Alerting

## Tổng quan

Observability Stack là **lớp giám sát Phase 7** của dự án EveryoneCook. Nó quản lý CloudWatch dashboards, alarms, và SNS notifications để giám sát toàn bộ hạ tầng.

**Thứ tự triển khai**: Stack này **NÊN** được triển khai cuối cùng sau tất cả các stacks khác.

### Trách nhiệm chính

- Tạo CloudWatch Dashboards cho visualization
- Cấu hình CloudWatch Alarms cho monitoring
- Thiết lập SNS Topic cho notifications
- Tạo Composite Alarm cho overall health

### Tài nguyên chính

- **CloudWatch Dashboards** (4 dashboards):
  - Core Dashboard: DynamoDB, S3, CloudFront, KMS
  - Auth Dashboard: Cognito, Lambda triggers
  - Backend Dashboard: API Gateway, Lambda, SQS, WAF
  - Overview Dashboard: System health summary
- **CloudWatch Alarms** (15+ alarms):
  - Critical: 5xx errors, Lambda errors, DLQ messages
  - Warning: 4xx errors, high latency, throttling
  - Cost: Budget exceeded alerts
- **Composite Alarm**: Overall system health
- **SNS Topic**: Email notifications

---

## Các bước triển khai

### Bước 1: Xác minh điều kiện tiên quyết

-  Tất cả stacks khác đã triển khai thành công

### Bước 2: Triển khai Observability Stack

```powershell
cd D:\Project_AWS\everyonecook\infrastructure

# Triển khai Observability Stack
npx cdk deploy EveryoneCook-dev-Observability --context environment=dev
```

### Bước 3: Xác minh trên AWS Console

#### Xác minh CloudWatch Dashboards

1. Vào **CloudWatch** > **Dashboards**
2. Tìm 4 dashboards với prefix `EveryoneCook-dev`

#### Xác minh CloudWatch Alarms

1. Vào **CloudWatch** > **Alarms**
2. Tìm 15+ alarms
3. Xác minh Composite Alarm

#### Xác minh SNS Topic

1. Vào **SNS** > **Topics**
2. Tìm topic `EveryoneCook-dev-Alarms`
3. Xác minh email subscription

---

## Phân tích chi phí

| Tài nguyên | Chi phí | Ghi chú |
|----------|---------|-------|
| **CloudWatch Dashboards** | $0-1/tháng | First 3 free |
| **CloudWatch Alarms** | $0.50-1/tháng | $0.10/alarm |
| **CloudWatch Logs** | $1-2/tháng | 7-day retention |
| **SNS** | $0/tháng | Email notifications free |
| **Tổng** | **~$3-8/tháng** | |

---

## Bước tiếp theo

➡️ **[5.05 Deploy Infrastructure](../../5.05-deploy-infrastructure/)** - Triển khai tất cả stacks lên AWS
