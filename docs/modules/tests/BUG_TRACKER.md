# Bug Tracker | Product Management System

This file logs all bugs discovered during testing. Each bug must be linked to a GitHub Issue.

---

## Active Bugs

| Bug ID          | Module   | TC Ref               | Severity   | Description                                   | GitHub Issue | Status     | Reported By | Reported On | Fixed By | Fixed On |
|-----------------|----------|----------------------|------------|-----------------------------------------------|--------------|------------|-------------|-------------|----------|----------|
| BUG-PM-001      | API      | AXN-PM-API-TC-000    | High       | No API key authentication any request with a valid db_name is accepted | *(Open Issue)* | OPEN ❌ | — | 2025-12-24 | — | — |
| BUG-PM-002      | SUB      | AXN-PM-SUB-TC-007    | Low        | One-time subscription hardcodes +10 years. No configurable duration | *(Open Issue)* | OPEN ❌ | — | 2025-12-24 | — | — |
| BUG-PM-003      | SUB      | —                    | Medium     | `transaction_no` generated using `time()+rand` not guaranteed unique under concurrent requests | *(Open Issue)* | OPEN ❌ | — | 2025-12-24 | — | — |
| BUG-PM-004      | DB       | —                    | Medium     | `db_password` in `customer_products` stored as plain text should be encrypted | *(Open Issue)* | OPEN ❌ | — | 2025-12-24 | — | — |

---

## Resolved Bugs

| Bug ID | Module | Description | Fixed In | Resolved On |
|--------|--------|-------------|----------|-------------|
| *(None yet)* | | | | |

---

## Instructions

1. Every `FAIL ❌` test case must produce a bug entry here.
2. Link the GitHub Issue number in the `GitHub Issue` column.
3. Update status as: `OPEN ❌` → `IN FIX 🔄` → `RESOLVED ✅`.
4. Developer who fixes the bug must update the test case `Status` field too.
5. QA re-runs the test case on staging before marking `RESOLVED`.
