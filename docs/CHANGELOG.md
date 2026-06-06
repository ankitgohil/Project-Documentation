# CHANGELOG — AXN-042 Axone Client Portal

All notable changes to this project are documented here.
Format: `[VERSION] YYYY-MM-DD — Developer — Description`

---

## [v2.4.0] — 2024-06-20 — Rahul Mehta
- Added Notifications module (in-app + push via FCM)
- Upgraded Laravel to v11.x
- Refactored Auth middleware to support refresh token rotation
- Fixed: Expired JWT not redirecting on mobile (#187) — *PARTIAL FIX, see #187*

## [v2.3.1] — 2024-06-10 — Arjun Patel
- Fixed: Reports export crash for large datasets (paginated export added) (#201)
- Added: CSV export option for all report types
- Updated: endpoints.md with new `/api/v1/reports/export` route

## [v2.3.0] — 2024-05-28 — Priya Sharma
- Dashboard: Added Revenue Chart widget (Recharts, monthly breakdown)
- Dashboard: Added Quick Stats cards (Active Users, Pending Tasks, Revenue MTD)
- Mobile: New Dashboard home screen with pull-to-refresh

## [v2.2.0] — 2024-05-10 — Rahul Mehta
- Integrated Razorpay payment gateway (Invoice module, Planned → In Progress)
- Added spatie/permission role management
- Admin Panel: User role assignment UI

## [v2.1.0] — 2024-04-15 — Priya Sharma
- Authentication: Added Google OAuth login
- Authentication: Implemented rate limiting (5 attempts/min per IP)
- Mobile: Login screen redesign

## [v2.0.0] — 2024-03-01 — Rahul Mehta
- Initial production release
- Modules: Auth, Dashboard, Admin Panel
- Laravel 10 baseline, Flutter 3.x baseline
