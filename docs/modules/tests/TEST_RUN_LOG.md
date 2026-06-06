# Test Run Log — AXN-042 Axone Client Portal

Sprint-wise test execution summary. Updated by QA at the end of each sprint.

---

## Sprint 8 — June 10–21, 2024

**Sprint Goal:** Notifications module MVP, Dashboard Revenue Chart, Auth Refresh Token Rotation

| Module | Total TCs | PASS | FAIL | PENDING | Pass Rate |
|---|---|---|---|---|---|
| Authentication | 14 | 12 | 1 | 1 | 85.7% |
| Dashboard | 7 | 7 | 0 | 0 | 100% |
| Notifications | 5 | 3 | 0 | 2 | 60% (2 blocked on FCM config) |
| **Sprint Total** | **26** | **22** | **1** | **3** | **84.6%** |

**Open P1 FAILs:**
- AXN042-AUTH-TC-006 → #187 (Expired JWT not redirecting on Flutter). Carried to Sprint 9.

**Blocked:**
- NOTIF TCs 4–5: FCM staging config not yet set up. Awaiting DevOps.

---

## Sprint 7 — May 27–June 7, 2024

**Sprint Goal:** Dashboard widget expansion, Reports CSV/XLSX export fix

| Module | Total TCs | PASS | FAIL | PENDING | Pass Rate |
|---|---|---|---|---|---|
| Dashboard | 5 | 5 | 0 | 0 | 100% |
| Reports | 8 | 7 | 1 | 0 | 87.5% |
| **Sprint Total** | **13** | **12** | **1** | **0** | **92.3%** |

**FAILs Resolved This Sprint:**
- AXN042-RPT-TC-005 (XLSX crash > 50k rows) → Fixed #201 in v2.3.1

---

## Sprint 6 — May 13–24, 2024

**Sprint Goal:** Google OAuth, Role-based permissions, Admin Panel basics

| Module | Total TCs | PASS | FAIL | PENDING | Pass Rate |
|---|---|---|---|---|---|
| Authentication | 9 | 9 | 0 | 0 | 100% |
| Admin | 6 | 5 | 0 | 1 | 83.3% |
| **Sprint Total** | **15** | **14** | **0** | **1** | **93.3%** |
