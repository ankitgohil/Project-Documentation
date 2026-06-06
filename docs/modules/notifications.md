# Module: Notifications (NOTIF)

---

## 1. Module Identity

| Field | Value |
|---|---|
| **Module Name** | Notifications |
| **Module ID** | NOTIF |
| **Platform** | Laravel API + Flutter |
| **Status** | In Progress |
| **Assigned Developer** | Sneha Joshi |
| **Creation Date** | 2024-06-01 |
| **Last Updated** | 2024-06-18 |

---

## 2. Purpose & Scope

The Notifications module delivers real-time in-app notifications and mobile push notifications (via Firebase FCM) to users for key events such as task assignments, report completions, and payment receipts. It does NOT handle email notifications (managed by Mailgun in respective modules) or SMS (managed by Twilio in Auth module).

---

## 3. User Roles & Permissions

| Role | Receive In-App | Receive Push | Manage Notification Settings | Mark All Read |
|---|---|---|---|---|
| Super Admin | ✅ | ✅ | ✅ | ✅ |
| Admin | ✅ | ✅ | ✅ | ✅ |
| Manager | ✅ | ✅ | ✅ (own only) | ✅ |
| User | ✅ | ✅ | ✅ (own only) | ✅ |

---

## 4. Feature List

- [DONE] In-app notification bell with unread count (Dashboard widget)
- [DONE] Notification list screen (paginated)
- [DONE] Mark single notification as read
- [DONE] Mark all notifications as read
- [DONE] Push notification delivery via Firebase FCM
- [IN PROGRESS] Notification preferences screen (opt in/out per type)
- [IN PROGRESS] Real-time unread count update via WebSocket (Pusher)
- [PLANNED] Notification history (30-day archive)
- [PLANNED] Grouped notifications (bundle multiple events)

---

## 5. API Endpoints

### GET /api/v1/notifications

| Field | Value |
|---|---|
| **Auth Required** | Yes (Bearer token) |
| **Description** | Returns paginated list of notifications for authenticated user |
| **Query Params** | `page` (int), `per_page` (int, max 50), `unread_only` (bool, default false) |
| **Success (200)** | `{ "data": [ { "id": "uuid", "type": "task_assigned", "message": "string", "read_at": null, "created_at": "ISO8601" } ], "meta": { "unread_count": 5, "total": 42 } }` |

### PATCH /api/v1/notifications/{id}/read

| Field | Value |
|---|---|
| **Auth Required** | Yes |
| **Description** | Marks a specific notification as read |
| **Success (200)** | `{ "message": "Notification marked as read" }` |
| **Error (404)** | `{ "message": "Notification not found" }` |

### POST /api/v1/notifications/mark-all-read

| Field | Value |
|---|---|
| **Auth Required** | Yes |
| **Description** | Marks all notifications as read for the authenticated user |
| **Success (200)** | `{ "message": "All notifications marked as read", "updated_count": 5 }` |

---

## 6. Database Tables

| Table | Key Columns | Notes |
|---|---|---|
| `notifications` | id (UUID), notifiable_type, notifiable_id, type, data (JSON), read_at, created_at | Laravel's built-in notifications table |
| `user_fcm_tokens` | id, user_id, token, device_type (ios/android), created_at | FCM device tokens per user |

---

## 7. Business Logic

### Notification Dispatch Flow
1. An event fires in another module (e.g., `TaskAssigned`, `ReportReady`).
2. Laravel event listener picks up the event.
3. Notification is queued via Laravel Queues (Horizon).
4. Queue worker dispatches: stores in `notifications` table (in-app) AND sends FCM push via Firebase SDK.
5. On push delivery failure (FCM error), log to `failed_jobs` table for retry.

### Push Notification Token Management
1. On mobile app login, Flutter app calls `POST /api/v1/notifications/register-token` with FCM token.
2. If token exists for this user+device: update. If new device: insert.
3. On logout, token is deleted from `user_fcm_tokens`.

---

## 8. Mobile Screens (Flutter)

| Screen Name | Route | Data Source | Key Interactions |
|---|---|---|---|
| Notification Bell | Dashboard overlay | WebSocket / GET /notifications | Tap opens Notification List |
| Notification List | `/notifications` | GET /api/v1/notifications | Scroll, tap to mark read, "Mark All Read" button |
| Notification Preferences | `/notifications/settings` | GET/POST /notifications/preferences | Toggle per notification type |

---

## 9. Error Handling

| Error | HTTP Code | User-Facing Message |
|---|---|---|
| Notification not found | 404 | "Notification not found." |
| FCM delivery failure | — | Silent failure, retry via queue. User notified in-app regardless. |
| Unauthenticated | 401 | Redirect to login |

---

## 10. GitHub References

- Branch pattern: `feature/notif-[task]`, `fix/notif-[issue]`
- Related PRs: #102 (In-app notifications), #108 (FCM push integration)
- Open Issues: #215 (Push notification delivery delay on iOS > 5 min)
- Milestone: v2.5.0 (Preferences screen, WebSocket real-time)

---

## 11. Change Log

| Date | Developer | Change |
|---|---|---|
| 2024-06-18 | Sneha Joshi | FCM push notification delivery integrated |
| 2024-06-10 | Sneha Joshi | In-app notifications API + Flutter list screen |
| 2024-06-01 | Sneha Joshi | Module stub created, database migrations added |
