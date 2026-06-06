# Module: User Dashboard (DASH)

---

## 1. Module Identity

| Field | Value |
|---|---|
| **Module Name** | User Dashboard |
| **Module ID** | DASH |
| **Platform** | Laravel API + Flutter |
| **Status** | Active |
| **Assigned Developer** | Priya Sharma |
| **Creation Date** | 2024-02-10 |
| **Last Updated** | 2024-06-10 |

---

## 2. Purpose & Scope

The Dashboard module provides users with a real-time overview of key metrics, recent activity, and quick-access widgets after login. It aggregates data from multiple modules (Reports, Notifications, Users) and presents it in a unified view. It does NOT handle detailed report generation (see reports.md) or notification settings (see notifications.md).

---

## 3. User Roles & Permissions

| Role | View Own Stats | View Team Stats | View Revenue Widget | Export Dashboard |
|---|---|---|---|---|
| Super Admin | ✅ | ✅ | ✅ | ✅ |
| Admin | ✅ | ✅ | ✅ | ✅ |
| Manager | ✅ | ✅ (own team) | ❌ | ✅ |
| User | ✅ | ❌ | ❌ | ❌ |

---

## 4. Feature List

- [DONE] Quick Stats cards (Active Users, Pending Tasks, Revenue MTD)
- [DONE] Revenue Chart (monthly breakdown, line chart)
- [DONE] Recent Activity Feed (last 10 actions by user/team)
- [DONE] Notification Bell with unread count badge
- [DONE] Pull-to-refresh (mobile)
- [IN PROGRESS] Date range filter for Revenue Chart
- [IN PROGRESS] Customizable widget layout (drag-to-reorder)
- [PLANNED] Export dashboard as PDF

---

## 5. API Endpoints

### GET /api/v1/dashboard/summary

| Field | Value |
|---|---|
| **Auth Required** | Yes (Bearer token) |
| **Description** | Returns all dashboard summary data in a single call |
| **Success (200)** | `{ "stats": { "active_users": 142, "pending_tasks": 17, "revenue_mtd": 284500.00 }, "revenue_chart": [ { "month": "Jan", "amount": 120000 }, ... ], "recent_activity": [ { "id": 1, "action": "string", "user": "string", "timestamp": "ISO8601" } ] }` |
| **Notes** | Response is cached in Redis for 5 minutes per user role. |

### GET /api/v1/dashboard/activity

| Field | Value |
|---|---|
| **Auth Required** | Yes (Bearer token) |
| **Description** | Returns paginated activity feed |
| **Query Params** | `page` (int, default 1), `per_page` (int, default 10, max 50) |
| **Success (200)** | `{ "data": [ ... ], "meta": { "current_page": 1, "total": 85 } }` |

---

## 6. Database Tables

| Table | Key Columns | Notes |
|---|---|---|
| `activity_logs` | id, user_id, action, entity_type, entity_id, created_at | All user actions logged here |
| `users` | id, name, role_id | Joined for activity feed display |

---

## 7. Business Logic

### Dashboard Summary Loading
1. Authenticate request.
2. Check Redis cache for `dashboard_summary_{user_role}` key (TTL: 5 min).
3. If cache miss: query DB for stats, revenue chart data, and recent activity.
4. Revenue MTD: SUM of `invoices.amount` where `status = 'paid'` and `paid_at >= first day of current month`.
5. Active users: COUNT of `users` where `last_login_at >= NOW() - 30 days` and `deleted_at IS NULL`.
6. Pending tasks: COUNT of `tasks` where `status = 'pending'` and `assigned_to = auth()->id()` (or all for Admin+).
7. Store in Redis cache and return response.

---

## 8. Mobile Screens (Flutter)

| Screen Name | Route | Data Source | Key Interactions |
|---|---|---|---|
| Dashboard Home | `/dashboard` | GET /dashboard/summary | Pull-to-refresh, tap stats card to navigate to module |
| Activity Feed | `/dashboard/activity` | GET /dashboard/activity | Infinite scroll pagination |

---

## 9. Error Handling

| Error | HTTP Code | User-Facing Message |
|---|---|---|
| Unauthenticated | 401 | Redirect to login |
| Insufficient role for widget | 403 | Widget hidden from UI (not shown as error) |
| Cache/DB failure | 500 | "Could not load dashboard. Please refresh." |

---

## 10. GitHub References

- Branch pattern: `feature/dashboard-[task]`, `fix/dashboard-[issue]`
- Related PRs: #77 (Revenue Chart), #83 (Quick Stats), #91 (Activity Feed)
- Open Issues: None currently
- Milestone: v2.5.0 (Date Range Filter, Widget Customization)

---

## 11. Change Log

| Date | Developer | Change |
|---|---|---|
| 2024-06-10 | Priya Sharma | Added Revenue Chart widget with monthly breakdown |
| 2024-05-28 | Priya Sharma | Added Quick Stats cards and Redis caching |
| 2024-04-01 | Priya Sharma | Initial Dashboard: activity feed, mobile home screen |
