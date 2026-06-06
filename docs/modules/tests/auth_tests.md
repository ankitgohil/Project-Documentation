# Test Cases — Authentication Module (AUTH)

**Project:** AXN-042 Axone Client Portal
**Module:** Authentication
**Test File Owner:** Karan Desai (QA)
**Last Updated:** 2024-06-20

---

## Test Case Table

| TC ID | Test Scenario | Type | Priority | Pre-Conditions | Test Input | Expected Result | Actual Result | Status | Tested By | Tested On | Bug Ref | Notes |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| **AXN042-AUTH-TC-001** | Login with valid email & password | Functional | P1 – Critical | User account exists with role: User | email: `test@axoneinfotech.com`, password: `Test@1234` | 200 OK. Dashboard loads. `access_token` and `refresh_token` returned in response. | As expected | **PASS ✅** | Karan Desai | 2024-06-15 | — | — |
| **AXN042-AUTH-TC-002** | Login with wrong password | Negative | P1 – Critical | User account exists | email: `test@axoneinfotech.com`, password: `WrongPass!` | 401 Unauthorized. Message: "Invalid credentials" | As expected | **PASS ✅** | Karan Desai | 2024-06-15 | — | — |
| **AXN042-AUTH-TC-003** | Login with empty email field | Validation | P2 – High | — | email: `""`, password: `Test@1234` | 422 Unprocessable. Error: `"email": ["The email field is required."]` | As expected | **PASS ✅** | Karan Desai | 2024-06-15 | — | — |
| **AXN042-AUTH-TC-004** | Login with malformed email (no @) | Validation | P2 – High | — | email: `testaxone.com`, password: `Test@1234` | 422 Unprocessable. Error: `"email": ["The email must be a valid email address."]` | As expected | **PASS ✅** | Karan Desai | 2024-06-15 | — | — |
| **AXN042-AUTH-TC-005** | Access protected endpoint without token | Security | P1 – Critical | — | GET `/dashboard/summary` with no Authorization header | 401 Unauthorized. Message: "Unauthenticated." | As expected | **PASS ✅** | Karan Desai | 2024-06-15 | — | — |
| **AXN042-AUTH-TC-006** | Use expired access token | Security | P1 – Critical | User logged in; access token manually expired in DB | GET `/dashboard/summary` with expired token | 401 Unauthorized. Mobile should redirect to login. | Mobile does NOT redirect — shows blank screen | **FAIL ❌** | Karan Desai | 2024-06-15 | #187 | Flutter interceptor not handling 401 on expired token. Backend response is correct. |
| **AXN042-AUTH-TC-007** | Login with SQL injection in email field | Security | P1 – Critical | — | email: `' OR '1'='1`, password: `anything` | 422 Validation error. No DB query executed. | As expected | **PASS ✅** | Karan Desai | 2024-06-15 | — | — |
| **AXN042-AUTH-TC-008** | Forgot Password — valid email | Functional | P2 – High | User account exists with verified email | email: `test@axoneinfotech.com` | 200 OK. Email sent with reset link. Link valid for 60 minutes. | Not yet tested on staging | **PENDING 🔄** | — | — | — | Awaiting Mailgun staging config |
| **AXN042-AUTH-TC-009** | Rate limit: 6 login attempts in 1 minute | Performance | P2 – High | Rate limiter active in config | 6 consecutive login attempts from same IP | 429 Too Many Requests on 6th attempt | As expected | **PASS ✅** | Karan Desai | 2024-06-15 | — | Verified with Postman rapid-fire requests |
| **AXN042-AUTH-TC-010** | Logout invalidates token on server | Functional | P1 – Critical | User logged in with valid token | POST `/auth/logout` then GET `/dashboard/summary` with old token | POST returns 200. Subsequent GET returns 401 Unauthorized. | As expected | **PASS ✅** | Karan Desai | 2024-06-15 | — | — |
| **AXN042-AUTH-TC-011** | Refresh token — valid refresh token | Functional | P1 – Critical | User logged in; access token expired | POST `/auth/refresh` with valid refresh_token | 200 OK. New `access_token` returned. Old access token invalidated. | As expected | **PASS ✅** | Karan Desai | 2024-06-18 | — | — |
| **AXN042-AUTH-TC-012** | Refresh token — invalid refresh token | Negative | P1 – Critical | — | POST `/auth/refresh` with random string as token | 401 Unauthorized. Message: "Invalid or expired refresh token" | As expected | **PASS ✅** | Karan Desai | 2024-06-18 | — | — |
| **AXN042-AUTH-TC-013** | Google OAuth — successful login | Functional | P2 – High | Google account exists; OAuth app configured on staging | Complete Google OAuth consent flow | User authenticated. JWT tokens returned. User created/linked in DB. | As expected | **PASS ✅** | Priya Sharma | 2024-06-01 | — | Tested via browser redirect flow |
| **AXN042-AUTH-TC-014** | Reset password — weak password | Validation | P2 – High | Valid reset token in DB | password: `abc`, password_confirmation: `abc` | 422. Error: "Password must be at least 8 characters." | As expected | **PASS ✅** | Karan Desai | 2024-06-18 | — | — |

---

## Test Summary

| Status | Count |
|---|---|
| PASS ✅ | 12 |
| FAIL ❌ | 1 |
| PENDING 🔄 | 1 |
| BLOCKED ⚠️ | 0 |
| SKIPPED — | 0 |
| **Total** | **14** |

**Open FAILs:** #187 (TC-006) — P1. Must resolve before v2.5.0 release.
