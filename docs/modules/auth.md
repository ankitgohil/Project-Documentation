# Module: Authentication

---

## 1.0 Module Identity

| Field            | Details                                   |
|------------------|-------------------------------------------|
| **Module Name**  | Authentication                            |
| **Module Code**  | AUTH                                      |
| **Platform**     | Laravel + React (Inertia)                 |
| **Status**       | Active                                    |
| **Developer**    | *(Assign)*                                |
| **Created Date** | 2025-12-13                                |

---

## 2.0 Purpose & Scope

The Authentication module handles all user identity verification for the **admin web panel**. It provides login, registration, password reset, email verification, and profile password update. This module governs access to all protected routes under the `auth` middleware. It does **not** cover external client API authentication (see `license_api.md`).

---

## 3.0 User Roles & Permissions

| Role  | Can Login | Can Register | Can Reset Password | Can Update Profile |
|-------|-----------|--------------|--------------------|--------------------|
| Admin | ✅         | ✅ (via register route) | ✅         | ✅                  |
| User  | ✅         | ✅           | ✅                  | ✅                  |
| Guest | ❌         | ✅           | ✅                  | ❌                  |

---

## 4.0 Feature List

- [DONE] Email + Password Login
- [DONE] User Registration
- [DONE] Forgot Password (email reset link)
- [DONE] Password Reset via token
- [DONE] Email Verification flow
- [DONE] Password update (authenticated)
- [DONE] Session Logout
- [DONE] Profile edit (name, email, password)
- [PLANNED] OAuth / Social Login

---

## 5.0 API Endpoints

> These are web routes (session-based), not REST API endpoints.

| Method | Route                          | Auth     | Controller                              | Purpose                   |
|--------|-------------------------------|----------|-----------------------------------------|---------------------------|
| GET    | `/login`                       | Guest    | `AuthenticatedSessionController@create` | Show login form            |
| POST   | `/login`                       | Guest    | `AuthenticatedSessionController@store`  | Process login              |
| POST   | `/logout`                      | Auth     | `AuthenticatedSessionController@destroy`| Logout user                |
| GET    | `/register`                    | Guest    | `RegisteredUserController@create`       | Show registration form     |
| POST   | `/register`                    | Guest    | `RegisteredUserController@store`        | Create new user            |
| GET    | `/forgot-password`             | Guest    | `PasswordResetLinkController@create`    | Show forgot password form  |
| POST   | `/forgot-password`             | Guest    | `PasswordResetLinkController@store`     | Send reset email           |
| GET    | `/reset-password/{token}`      | Guest    | `NewPasswordController@create`          | Show reset form            |
| POST   | `/reset-password`              | Guest    | `NewPasswordController@store`           | Process password reset     |
| PUT    | `/password`                    | Auth     | `PasswordController@update`             | Update current password    |

---

## 6.0 Database Tables

| Table   | Key Columns                                    | Notes                          |
|---------|------------------------------------------------|--------------------------------|
| `users` | `id`, `name`, `email`, `password`, `role_id`, `status`, `email_verified_at` | Core user identity |
| `password_reset_tokens` | `email`, `token`, `created_at`    | Token for password reset       |
| `personal_access_tokens` | `tokenable_id`, `tokenable_type`, `token` | Sanctum tokens (API)  |
| `sessions` | `id`, `user_id`, `ip_address`, `payload`   | Session storage                |

---

## 7.0 Business Logic

1. User submits email + password to `POST /login`.
2. Laravel validates credentials against `users` table (bcrypt comparison).
3. On success session is created, user is redirected to `/dashboard`.
4. On failure validation error returned, 5-attempt lockout recommended (not yet implemented).
5. Password reset sends a signed tokenized link to the user's email.
6. Token expires after 60 minutes (Laravel default).
7. Email verification sends a signed URL. The `verified` middleware blocks access until the email is verified.

---

## 8.0 Mobile Screens

N/A Web admin panel only.

---

## 9.0 Error Handling

| Scenario                   | HTTP Code | User Message                          |
|---------------------------|-----------|---------------------------------------|
| Invalid credentials        | 422       | "These credentials do not match our records." |
| Unverified email           | Redirect  | Redirected to `/verify-email` prompt  |
| Invalid reset token        | 422       | "This password reset token is invalid." |
| Validation failure         | 422       | Field-level error messages via Inertia |

---

## 10.0 GitHub References

- **Branch Pattern**: `feature/auth-[task-name]`, `fix/auth-[issue]`
- **Module Doc**: `docs/modules/auth.md`

---

## 11.0 Change Log

| Date       | Developer | Change                                     |
|------------|-----------|---------------------------------------------|
| 2025-12-13 | —         | Initial Auth module via Laravel Breeze setup |
| 2025-12-13 | —         | Added `role_id`, `phone`, `status` to users  |
