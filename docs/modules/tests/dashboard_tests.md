# Test Cases — Dashboard Module (DASH)

**Project:** AXN-042 Axone Client Portal
**Module:** User Dashboard
**Test File Owner:** Karan Desai (QA)
**Last Updated:** 2024-06-18

---

## Test Case Table

| TC ID | Test Scenario | Type | Priority | Pre-Conditions | Test Input | Expected Result | Actual Result | Status | Tested By | Tested On | Bug Ref | Notes |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| **AXN042-DASH-TC-001** | Load dashboard summary as Admin | Functional | P1 – Critical | Admin user logged in | GET `/dashboard/summary` | 200 OK. Returns stats, revenue_chart, recent_activity. All fields present. | As expected | **PASS ✅** | Karan Desai | 2024-06-18 | — | — |
| **AXN042-DASH-TC-002** | Load dashboard summary as User (role) | Functional | P1 – Critical | User (non-admin) logged in | GET `/dashboard/summary` | 200 OK. Revenue widget absent or zero for non-admin role. | As expected | **PASS ✅** | Karan Desai | 2024-06-18 | — | Revenue widget hidden based on role in Flutter |
| **AXN042-DASH-TC-003** | Dashboard loads within 2 seconds | Performance | P2 – High | Redis running, cache populated | GET `/dashboard/summary` (second request, cache hit) | Response time < 200ms (cached). | 145ms | **PASS ✅** | Karan Desai | 2024-06-18 | — | Tested via Postman |
| **AXN042-DASH-TC-004** | Dashboard data refreshes on pull-to-refresh (mobile) | Functional | P2 – High | Logged in on Flutter app | Pull down on dashboard screen | Activity feed updates. Stats refresh. | As expected | **PASS ✅** | Karan Desai | 2024-06-18 | — | Tested on Android emulator |
| **AXN042-DASH-TC-005** | Activity feed pagination | Functional | P2 – High | Logged in, > 20 activity entries in DB | GET `/dashboard/activity?page=2&per_page=10` | 200 OK. Returns records 11–20. Meta shows correct pagination. | As expected | **PASS ✅** | Karan Desai | 2024-06-18 | — | — |
| **AXN042-DASH-TC-006** | Access dashboard without auth token | Security | P1 – Critical | — | GET `/dashboard/summary` no token | 401 Unauthorized | As expected | **PASS ✅** | Karan Desai | 2024-06-18 | — | — |
| **AXN042-DASH-TC-007** | Revenue chart data correctness | Functional | P2 – High | Known invoice data seeded in DB | GET `/dashboard/summary` | revenue_chart data matches sum of invoices per month in DB | As expected | **PASS ✅** | Karan Desai | 2024-06-18 | — | Verified by cross-checking with raw SQL query |

---

## Test Summary

| Status | Count |
|---|---|
| PASS ✅ | 7 |
| FAIL ❌ | 0 |
| PENDING 🔄 | 0 |
| **Total** | **7** |
