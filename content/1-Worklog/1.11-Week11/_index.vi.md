---
title: "Worklog Tuần 11"
date: 2025-11-20
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---
## Tuần 11: Kiểm thử Frontend & Integration

### Mục tiêu Tuần 11:

* Kiểm thử frontend components và user flows (comments, friends, posts).
* Sửa UI/UX bugs và xác minh API integration với error handling.
* Kiểm thử session management, token refresh, và protected route redirects.

### Các công việc trong tuần:

| Ngày | Công việc                                                  | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo            |
| ---- | ---------------------------------------------------------- | ------------ | --------------- | ----------------------------- |
| 2    | Test Comments Service - TODO functions chưa implement      | 11/20/2025   | 11/20/2025      | frontend/services/comments.ts |
| 3    | Test Friend Request flow - Cancel/Accept/Reject            | 11/21/2025   | 11/21/2025      | frontend/services/friends.ts  |
| 4    | Test Post reactions - Like/Unlike toggle                   | 11/22/2025   | 11/22/2025      | frontend/services/posts.ts    |
| 5    | Test Session expired handling - Token refresh              | 11/23/2025   | 11/23/2025      | AuthContext.tsx               |
| 6    | Test Protected Routes - Redirect logic                     | 11/24/2025   | 11/24/2025      | ProtectedRoute.tsx            |

### Kết quả đạt được Tuần 11:

- Implement missing comment service functions (getComments, createComment, v.v.)
- Sửa friend request state management issues
- Xác minh token refresh mechanism hoạt động đúng
- Kiểm thử protected route redirects cho unauthenticated users

### Bugs tìm thấy & Đã sửa:

| Bug ID  | Mô tả                                                          | Mức độ     | Trạng thái |
| ------- | -------------------------------------------------------------- | ---------- | ---------- |
| BUG-004 | Comments service functions trả về empty - TODO chưa implement  | Cao        | Đã sửa     |
| BUG-005 | Friend request cancel không update UI state                    | Trung bình | Đã sửa     |
| BUG-006 | Token refresh interval gây memory leak                         | Trung bình | Đã sửa     |
| BUG-007 | Avatar cache không clear khi logout                            | Thấp       | Đã sửa     |
