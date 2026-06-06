# Bug Tracker — AXN-042 Axone Client Portal

Bugs found during QA testing. All entries must be linked to a GitHub Issue.

---

| Bug ID | GitHub Issue | Module | TC Ref | Severity | Title | Reported By | Reported On | Status | Fixed In | Notes |
|---|---|---|---|---|---|---|---|---|---|---|
| BUG-001 | #187 | Authentication | AUTH-TC-006 | P1 – Critical | Expired JWT token does not redirect to login on Flutter mobile | Karan Desai | 2024-06-15 | **OPEN** | — | Backend returns 401 correctly. Flutter HTTP interceptor not catching it. Assigned to Sneha Joshi. |
| BUG-002 | #201 | Reports | RPT-TC-005 | P2 – High | XLSX export crashes for datasets > 50,000 rows (memory limit) | Arjun Patel | 2024-05-28 | **RESOLVED** | v2.3.1 | Fixed with chunked XLSX export via OpenSpout. Tested with 80k rows. |
| BUG-003 | #215 | Notifications | NOTIF-TC-003 | P3 – Medium | Push notification delivery delay on iOS (> 5 minutes) | Sneha Joshi | 2024-06-18 | **OPEN** | — | FCM delivery confirmed on Android. iOS delay suspected to be APNs throttling. Under investigation. |

---

## Bug Status Definitions

| Status | Meaning |
|---|---|
| OPEN | Bug confirmed, not yet fixed |
| IN PROGRESS | Developer actively working on fix |
| RESOLVED | Fix deployed to staging and verified by QA |
| CLOSED | Fix deployed to production and confirmed |
| WONTFIX | Accepted risk or not reproducible |
