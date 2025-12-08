---
title: "Week 12 Worklog"
date: 2025-11-27
weight: 12
chapter: false
pre: " <b> 1.12. </b> "
---
## Week 12: AI Module & End-to-End Testing

### Week 12 Objectives:

* Test AI recipe suggestion flow and Bedrock integration.
* Fix friend-based privacy filtering and TypeScript errors.
* Perform end-to-end testing for the complete application.

### Tasks to be carried out this week:

| Day | Task                                               | Start Date | Completion Date | Reference Material     |
| --- | -------------------------------------------------- | ---------- | --------------- | ---------------------- |
| 2   | Test AI Suggestions - Cache matching logic         | 11/27/2025 | 11/27/2025      | suggestion.handler.ts  |
| 3   | Test Recipe CRUD - Create/Update/Delete            | 11/28/2025 | 11/28/2025      | ai-module/index.ts     |
| 4   | Fix TODO: Friend-based privacy filtering for posts | 11/29/2025 | 11/29/2025      | post.service.ts:757    |
| 5   | Fix TODO: Recipe-group handler TypeScript errors   | 11/30/2025 | 11/30/2025      | social-module/index.ts |
| 6   | End-to-end testing - Full user journey             | 12/01/2025 | 12/01/2025      | All modules            |

### Week 12 Achievements:

- Verified AI cache matching logic works correctly
- Fixed friend-based privacy filtering for feed posts
- Resolved TypeScript errors in recipe-group handler
- Completed full end-to-end testing

### Bugs Found & Fixed:

| Bug ID  | Description                                           | Severity | Status   |
| ------- | ----------------------------------------------------- | -------- | -------- |
| BUG-008 | AI cache rejected due to servings mismatch            | Medium   | Fixed    |
| BUG-009 | Friend posts not showing in feed (privacy filter bug) | High     | Fixed    |
| BUG-010 | Recipe-group handler TypeScript compilation errors    | Medium   | Fixed    |
| BUG-011 | S3 image deletion not queued on post delete           | Low      | Deferred |

---

## Summary of All TODOs Found in Codebase

| Location                      | TODO Description                                     | Priority |
| ----------------------------- | ---------------------------------------------------- | -------- |
| comment.service.ts:434        | Delete all replies when parent comment deleted       | High     |
| post.service.ts:757           | Add friend-based privacy filtering for friends posts | High     |
| social-module/index.ts:26     | Fix TypeScript errors in recipe-group.handler        | Medium   |
| moderation.service.ts:329     | Queue S3 image deletion (async via SQS)              | Low      |
| rate-limit.ts:99              | Send alert to admin team on rate limit               | Low      |
| frontend/services/comments.ts | Implement all TODO comment functions                 | High     |

---

## Testing Checklist

### Authentication & Authorization

- [ ] Login with valid credentials
- [ ] Login with invalid credentials
- [ ] Login with banned user
- [ ] Logout and session cleanup
- [ ] Token refresh before expiry
- [ ] Protected route redirect

### User Profile

- [ ] View own profile
- [ ] Update profile information
- [ ] Upload avatar
- [ ] Upload background
- [ ] Privacy settings update
- [ ] View other user's profile (with privacy)

### Social Features

- [ ] Create post (quick/recipe_share)
- [ ] Edit post title
- [ ] Delete post
- [ ] Like/Unlike post
- [ ] Comment on post
- [ ] Delete comment
- [ ] Report post/comment
- [ ] Share post

### Friends

- [ ] Search users
- [ ] Send friend request
- [ ] Accept friend request
- [ ] Reject friend request
- [ ] Cancel sent request
- [ ] Remove friend
- [ ] Block/Unblock user

### AI Features

- [ ] Get recipe suggestions
- [ ] Check job status
- [ ] Nutrition analysis
- [ ] Dictionary lookup

### Notifications

- [ ] Get notifications list
- [ ] Mark as read
- [ ] Mark all as read
- [ ] Delete notification
- [ ] Notification preferences

---

## Notes

1. **Critical Bug**: Comments service in frontend has TODO functions that return empty arrays - needs immediate implementation
2. **Performance**: Ban status check has retry logic with 5s timeout - may cause login delays
3. **Data Consistency**: Comment deletion doesn't cascade to replies - orphan data issue
4. **Privacy**: Friend-based post filtering not implemented in feed
5. **TypeScript**: Recipe-group handler commented out due to compilation errors
