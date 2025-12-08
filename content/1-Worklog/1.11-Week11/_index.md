---
title: "Week 11 Worklog"
date: 2025-11-20
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---
## Week 11: Frontend Testing & Integration

### Week 11 Objectives:

* Test frontend components and user flows (comments, friends, posts).
* Fix UI/UX bugs and verify API integration with error handling.
* Test session management, token refresh, and protected route redirects.

### Tasks to be carried out this week:

| Day | Task                                                   | Start Date | Completion Date | Reference Material            |
| --- | ------------------------------------------------------ | ---------- | --------------- | ----------------------------- |
| 2   | Test Comments Service - TODO functions not implemented | 11/20/2025 | 11/20/2025      | frontend/services/comments.ts |
| 3   | Test Friend Request flow - Cancel/Accept/Reject        | 11/21/2025 | 11/21/2025      | frontend/services/friends.ts  |
| 4   | Test Post reactions - Like/Unlike toggle               | 11/22/2025 | 11/22/2025      | frontend/services/posts.ts    |
| 5   | Test Session expired handling - Token refresh          | 11/23/2025 | 11/23/2025      | AuthContext.tsx               |
| 6   | Test Protected Routes - Redirect logic                 | 11/24/2025 | 11/24/2025      | ProtectedRoute.tsx            |

### Week 11 Achievements:

- Implemented missing comment service functions (getComments, createComment, etc.)
- Fixed friend request state management issues
- Verified token refresh mechanism works correctly
- Tested protected route redirects for unauthenticated users

### Bugs Found & Fixed:

| Bug ID  | Description                                                    | Severity | Status |
| ------- | -------------------------------------------------------------- | -------- | ------ |
| BUG-004 | Comments service functions return empty - TODO not implemented | High     | Fixed  |
| BUG-005 | Friend request cancel not updating UI state                    | Medium   | Fixed  |
| BUG-006 | Token refresh interval causing memory leak                     | Medium   | Fixed  |
| BUG-007 | Avatar cache not cleared on logout                             | Low      | Fixed  |
