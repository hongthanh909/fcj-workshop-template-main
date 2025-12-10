---
title: "Worklog Tuần 10"
date: 2025-11-13
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---
## Tuần 10: Kiểm thử Backend & Sửa lỗi

### Mục tiêu Tuần 10:

* Kiểm thử backend Lambda functions và API endpoints.
* Sửa bugs authentication, authorization, và DynamoDB data consistency.
* Xác minh comment deletion cascade và profile data mapping.

### Các công việc trong tuần:

| Ngày | Công việc                                                       | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo     |
| ---- | --------------------------------------------------------------- | ------------ | --------------- | ---------------------- |
| 2    | Test Auth Module - Login/Logout flow                            | 11/13/2025   | 11/13/2025      | services/auth-module   |
| 3    | Test Ban Status API - Kiểm tra phát hiện user bị ban            | 11/14/2025   | 11/14/2025      | useBanStatus.ts        |
| 4    | Fix TODO: Xóa tất cả replies khi parent comment bị xóa          | 11/15/2025   | 11/15/2025      | comment.service.ts:434 |
| 5    | Test Profile CRUD operations - snake_case/camelCase mapping     | 11/16/2025   | 11/16/2025      | auth-module/index.ts   |
| 6    | Test Privacy settings - Xử lý Legacy boolean structure          | 11/17/2025   | 11/17/2025      | profile.service.ts:145 |

### Kết quả đạt được Tuần 10:

- Kiểm thử authentication flow bao gồm phát hiện banned user
- Sửa comment deletion cascade issue (replies không bị xóa)
- Xác minh profile data mapping giữa API và internal models
- Kiểm thử privacy settings migration từ legacy format

### Bugs tìm thấy & Đã sửa:

| Bug ID  | Mô tả                                                   | Mức độ | Trạng thái |
| ------- | ------------------------------------------------------- | ------ | ---------- |
| BUG-001 | Comment replies không bị xóa khi parent comment bị xóa  | Cao    | Đã sửa     |
| BUG-002 | Legacy privacy boolean fields không convert đúng        | Trung bình | Đã sửa |
| BUG-003 | Ban status check timeout gây login delays               | Trung bình | Đã sửa |
