# Authentication Module | Test Cases

**Project Code:** AXN-PM-01  
**Module Code:** AUTH  
**File Path:** `/docs/modules/tests/auth_tests.md`

---

## Test Case Summary Table

| TC ID               | Test Scenario                              | Type        | Priority    | Expected Result                                | Status     |
|---------------------|---------------------------------------------|-------------|-------------|------------------------------------------------|------------|
| AXN-PM-AUTH-TC-001  | Login with valid email & password           | Functional  | P1-Critical | Dashboard loads, session created               | PENDING 🔄 |
| AXN-PM-AUTH-TC-002  | Login with wrong password                  | Negative    | P1-Critical | 422 error, "These credentials do not match"   | PENDING 🔄 |
| AXN-PM-AUTH-TC-003  | Login with empty email field               | Validation  | P2-High     | 422 email field required                     | PENDING 🔄 |
| AXN-PM-AUTH-TC-004  | Login with invalid email format (no @)     | Validation  | P2-High     | 422 invalid email format                     | PENDING 🔄 |
| AXN-PM-AUTH-TC-005  | Access /dashboard without login            | Security    | P1-Critical | Redirect to /login                             | PENDING 🔄 |
| AXN-PM-AUTH-TC-006  | Logout invalidates session                 | Functional  | P1-Critical | Subsequent request → redirected to /login      | PENDING 🔄 |
| AXN-PM-AUTH-TC-007  | Forgot password with valid email           | Functional  | P2-High     | Reset link sent, 302 redirect with success msg | PENDING 🔄 |
| AXN-PM-AUTH-TC-008  | Forgot password with non-existent email    | Negative    | P2-High     | Validation error email not found             | PENDING 🔄 |
| AXN-PM-AUTH-TC-009  | Password reset with valid token            | Functional  | P1-Critical | Password updated, redirect to login            | PENDING 🔄 |
| AXN-PM-AUTH-TC-010  | Password reset with expired/invalid token  | Security    | P1-Critical | 422 invalid token message                    | PENDING 🔄 |
| AXN-PM-AUTH-TC-011  | Login with SQL injection in email field    | Security    | P1-Critical | 422 validation error, no DB execution          | PENDING 🔄 |
| AXN-PM-AUTH-TC-012  | Access route after session expires         | Security    | P2-High     | Redirect to login page                         | PENDING 🔄 |

---

## Detailed Test Cases

---

### AXN-PM-AUTH-TC-001

| Field             | Details                                                      |
|-------------------|--------------------------------------------------------------|
| **Test Case ID**  | AXN-PM-AUTH-TC-001                                           |
| **Module**        | Authentication                                               |
| **Feature**       | Login with Email & Password                                  |
| **Test Type**     | Functional                                                   |
| **Priority**      | P1 Critical                                                |
| **Pre-Conditions**| A user account exists in the `users` table with status=1     |
| **Test Steps**    | 1. Open `/login` 2. Enter valid email 3. Enter correct password 4. Click Login |
| **Test Input**    | email: `admin@axone.com` \| password: `Admin@1234`           |
| **Expected Result**| Redirect to `/dashboard`, session cookie set               |
| **Actual Result** | _(Fill after execution)_                                     |
| **Status**        | PENDING 🔄                                                   |
| **Tested By**     | —                                                            |
| **Tested On**     | —                                                            |
| **Bug Ref**       | —                                                            |

---

### AXN-PM-AUTH-TC-005

| Field             | Details                                                      |
|-------------------|--------------------------------------------------------------|
| **Test Case ID**  | AXN-PM-AUTH-TC-005                                           |
| **Module**        | Authentication                                               |
| **Feature**       | Route Protection                                             |
| **Test Type**     | Security                                                     |
| **Priority**      | P1 Critical                                                |
| **Pre-Conditions**| User is not logged in (no active session)                    |
| **Test Steps**    | 1. Open browser with no active session 2. Navigate to `/dashboard` |
| **Test Input**    | Direct URL: `/dashboard`                                     |
| **Expected Result**| HTTP 302 redirect to `/login`                              |
| **Actual Result** | _(Fill after execution)_                                     |
| **Status**        | PENDING 🔄                                                   |

---

### AXN-PM-AUTH-TC-011

| Field             | Details                                                      |
|-------------------|--------------------------------------------------------------|
| **Test Case ID**  | AXN-PM-AUTH-TC-011                                           |
| **Module**        | Authentication                                               |
| **Feature**       | SQL Injection Prevention                                     |
| **Test Type**     | Security                                                     |
| **Priority**      | P1 Critical                                                |
| **Pre-Conditions**| Login page is accessible                                     |
| **Test Steps**    | 1. Open `/login` 2. Enter SQL string in email: `' OR '1'='1'` 3. Enter any password 4. Submit |
| **Test Input**    | email: `' OR '1'='1'` \| password: `anything`               |
| **Expected Result**| 422 Validation error not a valid email format. No DB query executed. |
| **Actual Result** | _(Fill after execution)_                                     |
| **Status**        | PENDING 🔄                                                   |
