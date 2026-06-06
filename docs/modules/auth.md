# Module: Authentication (AUTH)

---

## 1. Module Identity

| Field | Value |
|---|---|
| **Module Name** | Authentication |
| **Module ID** | AUTH |
| **Platform** | Laravel API + Flutter |
| **Status** | Active |
| **Assigned Developer** | Priya Sharma |
| **Creation Date** | 2024-01-20 |
| **Last Updated** | 2024-06-01 |

---

## 2. Purpose & Scope

The Authentication module handles all user identity verification, session management, and access control for the Axone Client Portal. It serves all roles — Super Admin, Admin, Manager, User, and Guest. This module does NOT manage role-specific permissions (see admin.md) or user profile management.

---

## 3. User Roles & Permissions

| Role | Login | Register | Reset Password | View Own Profile | Manage Users |
|---|---|---|---|---|---|
| Super Admin | ✅ | ❌ (assigned only) | ✅ | ✅ | ✅ |
| Admin | ✅ | ❌ | ✅ | ✅ | ✅ |
| Manager | ✅ | ❌ | ✅ | ✅ | ❌ |
| User | ✅ | ❌ | ✅ | ✅ | ❌ |
| Guest | ❌ | ❌ | ❌ | ❌ | ❌ |

---

## 4. Feature List

- [DONE] Email + Password Login
- [DONE] Google OAuth Login
- [DONE] JWT Access Token issuance (60-min expiry)
- [DONE] Refresh Token (30-day expiry, rotation enabled)
- [DONE] Logout (server-side token invalidation)
- [DONE] Forgot Password (email link)
- [DONE] Reset Password
- [DONE] Rate Limiting (5 attempts/min per IP)
- [IN PROGRESS] Biometric Login (Flutter — fingerprint/Face ID)
- [PLANNED] Two-Factor Authentication (TOTP)

---

## 5. API Endpoints

### POST /api/v1/auth/login

| Field | Value |
|---|---|
| **Auth Required** | No (Public) |
| **Description** | Authenticates user with email and password, returns access + refresh tokens |
| **Request Headers** | `Content-Type: application/json` |
| **Request Body** | `{ "email": "string", "password": "string" }` |
| **Success (200)** | `{ "access_token": "string", "refresh_token": "string", "expires_in": 3600, "user": { "id": 1, "name": "string", "role": "string" } }` |
| **Error (401)** | `{ "message": "Invalid credentials" }` |
| **Error (422)** | `{ "errors": { "email": ["The email field is required."] } }` |
| **Error (429)** | `{ "message": "Too Many Requests" }` |
| **Notes** | Rate limited: 5 attempts per minute per IP. Token uses Laravel Sanctum. |

### POST /api/v1/auth/refresh

| Field | Value |
|---|---|
| **Auth Required** | No (Refresh token in body) |
| **Description** | Issues a new access token using a valid refresh token |
| **Request Body** | `{ "refresh_token": "string" }` |
| **Success (200)** | `{ "access_token": "string", "expires_in": 3600 }` |
| **Error (401)** | `{ "message": "Invalid or expired refresh token" }` |

### POST /api/v1/auth/logout

| Field | Value |
|---|---|
| **Auth Required** | Yes (Bearer token) |
| **Description** | Invalidates the current access token and refresh token on the server |
| **Success (200)** | `{ "message": "Logged out successfully" }` |

### POST /api/v1/auth/forgot-password

| Field | Value |
|---|---|
| **Auth Required** | No |
| **Request Body** | `{ "email": "string" }` |
| **Success (200)** | `{ "message": "Password reset link sent to your email" }` |
| **Notes** | Link valid for 60 minutes. Uses Mailgun for delivery. |

### POST /api/v1/auth/reset-password

| Field | Value |
|---|---|
| **Auth Required** | No (token from email link) |
| **Request Body** | `{ "token": "string", "email": "string", "password": "string", "password_confirmation": "string" }` |
| **Success (200)** | `{ "message": "Password reset successfully" }` |
| **Error (422)** | `{ "errors": { "password": ["Minimum 8 characters required."] } }` |

---

## 6. Database Tables

| Table | Key Columns | Notes |
|---|---|---|
| `users` | id, name, email, password, role_id, email_verified_at, created_at | Soft deletes enabled |
| `personal_access_tokens` | id, tokenable_id, name, token (hashed), abilities, last_used_at | Sanctum tokens |
| `password_reset_tokens` | email, token, created_at | Deleted after use |
| `oauth_providers` | id, user_id, provider (google), provider_user_id, token | Google OAuth links |

---

## 7. Business Logic

### Login Flow
1. Receive `email` and `password` from request.
2. Validate inputs (email format, required fields).
3. Check rate limit: if > 5 attempts from IP in last 60 seconds → return 429.
4. Find user by email in `users` table.
5. Verify password with `Hash::check()`.
6. If invalid → increment attempt counter → return 401.
7. If valid → generate Sanctum access token (60 min TTL) + refresh token (30 days).
8. Return tokens and user data.

### Refresh Token Flow
1. Receive `refresh_token` from request.
2. Validate token exists in `personal_access_tokens` and is not expired.
3. Invalidate the old refresh token (rotation).
4. Issue a new access token.
5. Return new access token.

### Logout Flow
1. Authenticate request via Bearer token middleware.
2. Call `$request->user()->currentAccessToken()->delete()` to invalidate token.
3. Also delete associated refresh token.
4. Return success message.

---

## 8. Mobile Screens (Flutter)

| Screen Name | Route | Data Source | Key Interactions |
|---|---|---|---|
| Login Screen | `/login` | — | Email/password form, Google OAuth button, "Forgot Password" link |
| Forgot Password Screen | `/forgot-password` | — | Email input, submit triggers API call |
| Reset Password Screen | `/reset-password` | Deep link token | Password + confirm fields |
| Biometric Prompt | Overlay (not a route) | Local biometric | Triggers on app resume if session exists |

---

## 9. Error Handling

| Error | HTTP Code | User-Facing Message |
|---|---|---|
| Invalid credentials | 401 | "Invalid email or password. Please try again." |
| Account not found | 404 | "No account found with this email." |
| Rate limit exceeded | 429 | "Too many attempts. Please wait 1 minute." |
| Token expired | 401 | "Your session has expired. Please log in again." |
| Validation failure | 422 | Field-specific error messages (e.g., "Email is required") |
| Server error | 500 | "Something went wrong. Please contact support." |

---

## 10. GitHub References

- Branch pattern: `feature/auth-[task]`, `fix/auth-[issue]`
- Related PRs: #45 (Google OAuth), #62 (Rate Limiting), #88 (Refresh Token Rotation)
- Open Issues: #187 (Expired JWT not redirecting on mobile — P1)
- Milestone: v2.5.0 (2FA Implementation)

---

## 11. Change Log

| Date | Developer | Change |
|---|---|---|
| 2024-06-01 | Priya Sharma | Added refresh token rotation logic |
| 2024-05-10 | Priya Sharma | Added Google OAuth via Socialite |
| 2024-04-20 | Priya Sharma | Implemented rate limiting middleware |
| 2024-03-01 | Priya Sharma | Initial Auth module: login, logout, forgot/reset password |
