# Messenger REST API: Software Testing and Quality Assurance Project

Manual API testing of a messenger application backend (users, authentication, conversations and messages), completed for the Software Testing and Quality Assurance course at United International University (UIU), Department of Computer Science and Engineering.

**Group 04, Section B ("100G")**

| Member | Student ID |
|--------|-----------|
| Shekh Rifat Islam | 011213036 |
| Arshad Md. Adel | 011221299 |
| MD. Shoaib Siddiqi Shaon | 011213164 |
| Al Shahrier Khan Nipu | 011202173 |
| Jannatul Ferdous Swarna | 011201157 |

## Overview

The system under test is a Java backend (started with Gradle) running on `http://localhost:8080`. It provides sign up and login with JWT tokens, user management, blocking, group and direct conversations, and messaging. This repository documents the test plan, the executed test cases and the defects found.

## Results at a Glance

| Item | Count |
|------|-------|
| Test cases | 20 (TC-01 to TC-20) |
| Executed scenarios | 77 |
| Passed | 47 |
| Failed (defects) | 30 |
| Defect reports | 15 (D-01 to D-15) |

Scenario counts are taken from the test case tables in the project report.

## API Endpoints Under Test

| Area | Method and endpoint | Test case |
|------|---------------------|-----------|
| Health | `GET /health` | TC-01 |
| Reset | `GET /reset` | TC-02 |
| Sign up | `POST /auth/signup` | TC-03 |
| Log in | `POST /auth/login` | TC-04 |
| Log out | `POST /auth/logout?username={username}` | TC-05 |
| Token check | `GET /auth/test` | TC-06 |
| All users | `GET /users/` | TC-07 |
| User by ID | `GET /users/{id}` | TC-08 |
| User by username | `GET /users/{username}` | TC-09 |
| Update user | `PATCH /users/{id}` | TC-10 |
| Block user | `PATCH /users/block/{id}` | TC-11 |
| Unblock user | `DELETE /users/block/{id}` | TC-12 |
| Create DIRECT conversation | `POST /conversations/create` | TC-13 |
| Create GROUP conversation | `POST /conversations/create` | TC-14 |
| My conversations | `GET /conversations/get` | TC-15 |
| Conversation by ID | `GET /conversations/{conversationId}` | TC-16 |
| Add members | `POST /conversations/addMember` | TC-17 |
| Remove members | `POST /conversations/removeMember` | TC-18 |
| Send message | `POST /messages/{conversationId}/send` | TC-19 |
| Get messages | `GET /messages/{conversationId}/get?page=0&size=20` | TC-20 |

Protected endpoints use the header `Authorization: Bearer <JWT_TOKEN>`.

## Test Design

- **Equivalence partitioning:** each input has representative values (for example token: empty, invalid, valid; username: null, existent, non-existent).
- **Negative testing:** null and empty fields, wrong or missing tokens, non-existent IDs, repeated actions.
- **Business-rule checks:** self-blocking, self-conversation, blocked users in conversations, member counts in direct conversations.
- **Evidence:** each scenario has a screen recording linked in the full report.

## Test Environment

| Item | Details |
|------|---------|
| Server | Run locally with Gradle on port 8080 |
| Base URL | `http://localhost:8080` |
| Reset between runs | `GET /reset` restores the initial data |
| Test tool | Postman (update if you used a different client) |

## How to Reproduce

1. Start the server with Gradle and confirm `GET /health` returns `200 OK`.
2. Call `GET /reset` to start from clean data.
3. Sign up a user with `POST /auth/signup`, then log in with `POST /auth/login` to get a JWT token.
4. Add the token as a Bearer token and run the scenarios in the order of the test cases (TC-01 to TC-20).
5. Compare each response with the expected status in the test case tables.

## Defect Summary

| ID | Defect | Severity |
|----|--------|----------|
| D-01 | Null or empty username, password or email accepted at sign up | Critical |
| D-02 | Unverified or dummy email accepted | High |
| D-03 | Logout works for users who are not logged in or do not exist | Medium |
| D-04 | Email updated without verification | High |
| D-05 | Profile update accepts empty username, email and password | Critical |
| D-06 | User can block their own account | Medium |
| D-07 | Direct conversation can be created with self | Low |
| D-08 | Direct conversation accepts multiple users | Medium |
| D-09 | Direct conversation created with a non-existent user | High |
| D-10 | Conversation type can be null or empty | Medium |
| D-11 | Group conversation created without a name | Medium |
| D-12 | Conversation created with a blocked user | High |
| D-13 | Non-existent user added as a conversation member | High |
| D-14 | Blocked user added to a group conversation | High |
| D-15 | Multiple members added to a direct conversation | Medium |

Other failures recorded in the test cases include `500 Internal Server Error` instead of `401` or `400` when calling conversation and message endpoints without authentication (TC-15, TC-16, TC-20), and removing a member who is not in the conversation (TC-18).

## Key Risks and Recommendations

1. **Broken input validation:** the API accepts null and empty values for core fields. Validate every input and return `400 Bad Request` with a clear message.
2. **Authentication and authorization gaps:** accounts can be created with dummy emails, logout works without login, and email can be changed without verification. Require email verification and authentication for sensitive actions.
3. **Broken identity and conversation model:** users can block themselves, chat with themselves, and be put in conversations with blocked users. A direct chat must always have exactly two different valid users, and blocking rules must be enforced everywhere.
4. **Wrong error codes:** unauthenticated requests return `500` instead of `401`. Server errors from bad client input hide real faults and should be fixed.

## Skills Demonstrated

- REST API functional and negative testing
- Equivalence partitioning and test case design
- JWT authentication and authorization testing
- HTTP status code and response analysis
- Clear, reproducible defect reporting with severity
- Risk analysis and recommendations

## Repository Contents

```
README.md          Project overview (this file)
docs/              Full project report (test plan, test cases, defect reports)
```

Update this section to match the actual files in your repository.
