---
title: "Week 10 Worklog"
date: 2025-11-13
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---
### Week 10 Objectives:

* Test backend Lambda functions and API endpoints.
* Fix authentication, authorization, and DynamoDB data consistency bugs.
* Verify comment deletion cascade and profile data mapping.

# Worklog - Testing & Bug Fixing Phase Week 10

## Week 10: Backend Testing & Bug Fixes

### Week 10 Objectives:

- Test backend Lambda functions và API endpoints
- Fix bugs authentication và authorization
- Verify DynamoDB operations và data consistency

### Tasks to be carried out this week:

| Day | Task                                                        | Start Date | Completion Date | Reference Material     |
| --- | ----------------------------------------------------------- | ---------- | --------------- | ---------------------- |
| 2   | Test Auth Module - Login/Logout flow                        | 11/13/2025 | 11/13/2025      | services/auth-module   |
| 3   | Test Ban Status API - Check user ban detection              | 11/14/2025 | 11/14/2025      | useBanStatus.ts        |
| 4   | Fix TODO: Delete all replies when parent comment deleted    | 11/15/2025 | 11/15/2025      | comment.service.ts:434 |
| 5   | Test Profile CRUD operations - snake_case/camelCase mapping | 11/16/2025 | 11/16/2025      | auth-module/index.ts   |
| 6   | Test Privacy settings - Legacy boolean structure handling   | 11/17/2025 | 11/17/2025      | profile.service.ts:145 |

### Week 10 Achievements:

- Tested authentication flow including banned user detection
- Fixed comment deletion cascade issue (replies not deleted)
- Verified profile data mapping between API and internal models
- Tested privacy settings migration from legacy format

### Bugs Found & Fixed:

| Bug ID  | Description                                             | Severity | Status |
| ------- | ------------------------------------------------------- | -------- | ------ |
| BUG-001 | Comment replies not deleted when parent comment deleted | High     | Fixed  |
| BUG-002 | Legacy privacy boolean fields not properly converted    | Medium   | Fixed  |
| BUG-003 | Ban status check timeout causing login delays           | Medium   | Fixed  |
