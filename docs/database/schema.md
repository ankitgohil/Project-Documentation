# Database Schema — AXN-042 Axone Client Portal

**Engine:** MySQL 8.0
**Charset:** utf8mb4
**Collation:** utf8mb4_unicode_ci

---

## Entity Relationship Overview

```
users ──< personal_access_tokens
users ──< oauth_providers
users ──< activity_logs
users ──< notifications
users ──< user_fcm_tokens
users ──< reports
users ──< tasks
users >── roles (via model_has_roles)
```

---

## Table Definitions

### `users`

| Column | Type | Nullable | Default | Notes |
|---|---|---|---|---|
| id | BIGINT UNSIGNED | No | AUTO_INCREMENT | PK |
| name | VARCHAR(255) | No | — | |
| email | VARCHAR(255) | No | — | UNIQUE |
| email_verified_at | TIMESTAMP | Yes | NULL | |
| password | VARCHAR(255) | No | — | bcrypt hashed |
| remember_token | VARCHAR(100) | Yes | NULL | |
| last_login_at | TIMESTAMP | Yes | NULL | Updated on each login |
| created_at | TIMESTAMP | No | CURRENT_TIMESTAMP | |
| updated_at | TIMESTAMP | No | CURRENT_TIMESTAMP | |
| deleted_at | TIMESTAMP | Yes | NULL | Soft deletes |

---

### `personal_access_tokens` (Laravel Sanctum)

| Column | Type | Nullable | Notes |
|---|---|---|---|
| id | BIGINT UNSIGNED | No | PK |
| tokenable_type | VARCHAR(255) | No | Polymorphic (App\Models\User) |
| tokenable_id | BIGINT UNSIGNED | No | FK → users.id |
| name | VARCHAR(255) | No | "access_token" or "refresh_token" |
| token | VARCHAR(64) | No | SHA-256 hash, UNIQUE |
| abilities | TEXT | Yes | JSON array of abilities |
| last_used_at | TIMESTAMP | Yes | |
| expires_at | TIMESTAMP | Yes | Set for access tokens |
| created_at | TIMESTAMP | No | |
| updated_at | TIMESTAMP | No | |

---

### `notifications` (Laravel built-in)

| Column | Type | Nullable | Notes |
|---|---|---|---|
| id | CHAR(36) | No | UUID, PK |
| type | VARCHAR(255) | No | Notification class name |
| notifiable_type | VARCHAR(255) | No | Polymorphic |
| notifiable_id | BIGINT UNSIGNED | No | FK → users.id |
| data | TEXT | No | JSON payload |
| read_at | TIMESTAMP | Yes | NULL = unread |
| created_at | TIMESTAMP | No | |
| updated_at | TIMESTAMP | No | |

**Index:** `(notifiable_type, notifiable_id)`

---

### `activity_logs`

| Column | Type | Nullable | Notes |
|---|---|---|---|
| id | BIGINT UNSIGNED | No | PK |
| user_id | BIGINT UNSIGNED | No | FK → users.id |
| action | VARCHAR(255) | No | e.g. "created_report", "logged_in" |
| entity_type | VARCHAR(255) | Yes | e.g. "App\Models\Report" |
| entity_id | BIGINT UNSIGNED | Yes | Related record ID |
| meta | JSON | Yes | Extra context |
| created_at | TIMESTAMP | No | |

---

### `reports`

| Column | Type | Nullable | Notes |
|---|---|---|---|
| id | CHAR(36) | No | UUID, PK |
| user_id | BIGINT UNSIGNED | No | FK → users.id |
| type | ENUM('revenue','activity','tasks') | No | |
| format | ENUM('csv','xlsx','pdf') | No | |
| status | ENUM('queued','processing','complete','failed') | No | Default: 'queued' |
| date_from | DATE | No | |
| date_to | DATE | No | |
| file_path | VARCHAR(500) | Yes | S3 URL |
| created_at | TIMESTAMP | No | |
| updated_at | TIMESTAMP | No | |

---

### `user_fcm_tokens`

| Column | Type | Nullable | Notes |
|---|---|---|---|
| id | BIGINT UNSIGNED | No | PK |
| user_id | BIGINT UNSIGNED | No | FK → users.id |
| token | TEXT | No | Firebase FCM token |
| device_type | ENUM('ios','android') | No | |
| created_at | TIMESTAMP | No | |
| updated_at | TIMESTAMP | No | |

**Unique Index:** `(user_id, device_type)` — one token per user per platform

---

## Conventions

1. All tables use soft deletes (`deleted_at`) where data retention is important.
2. UUIDs used as PK for publicly exposed resources (notifications, reports) to prevent enumeration.
3. All foreign keys have `ON DELETE CASCADE` unless soft deletes are in use.
4. JSON columns (meta, data) use MySQL's native JSON type for query support.
