# API Endpoints Reference — AXN-042

**Base URL (Staging):** `https://staging-portal.axoneinfotech.com/api/v1`
**Base URL (Production):** `https://portal.axoneinfotech.com/api/v1`
**Auth:** All protected routes require `Authorization: Bearer {token}` header.
**Content-Type:** All requests: `Content-Type: application/json`

---

## Authentication

| Method | Route | Auth | Description | Module Doc |
|---|---|---|---|---|
| POST | `/auth/login` | ❌ | Login with email + password | auth.md |
| POST | `/auth/refresh` | ❌ | Refresh access token | auth.md |
| POST | `/auth/logout` | ✅ | Logout, invalidate token | auth.md |
| POST | `/auth/forgot-password` | ❌ | Send password reset email | auth.md |
| POST | `/auth/reset-password` | ❌ | Reset password via token | auth.md |
| GET | `/auth/google/redirect` | ❌ | Initiate Google OAuth | auth.md |
| GET | `/auth/google/callback` | ❌ | Google OAuth callback | auth.md |

---

## Dashboard

| Method | Route | Auth | Description | Module Doc |
|---|---|---|---|---|
| GET | `/dashboard/summary` | ✅ | Full dashboard summary (stats, chart, activity) | dashboard.md |
| GET | `/dashboard/activity` | ✅ | Paginated activity feed | dashboard.md |

---

## Reports

| Method | Route | Auth | Description | Module Doc |
|---|---|---|---|---|
| POST | `/reports/generate` | ✅ | Queue a report generation job | reports.md |
| GET | `/reports/status/{job_id}` | ✅ | Poll report job status | reports.md |
| GET | `/reports` | ✅ | List all generated reports | reports.md |

---

## Notifications

| Method | Route | Auth | Description | Module Doc |
|---|---|---|---|---|
| GET | `/notifications` | ✅ | List notifications (paginated) | notifications.md |
| PATCH | `/notifications/{id}/read` | ✅ | Mark single notification as read | notifications.md |
| POST | `/notifications/mark-all-read` | ✅ | Mark all notifications as read | notifications.md |
| POST | `/notifications/register-token` | ✅ | Register FCM push token | notifications.md |

---

## Standard Response Formats

### Success
```json
{
  "data": { ... },
  "message": "optional string"
}
```

### Paginated Success
```json
{
  "data": [ ... ],
  "meta": {
    "current_page": 1,
    "per_page": 10,
    "total": 85,
    "last_page": 9
  }
}
```

### Validation Error (422)
```json
{
  "errors": {
    "field_name": ["Error message here."]
  }
}
```

### Generic Error
```json
{
  "message": "Human-readable error description."
}
```
