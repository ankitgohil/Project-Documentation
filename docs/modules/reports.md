# Module: Reports (RPT)

---

## 1. Module Identity

| Field | Value |
|---|---|
| **Module Name** | Reports Module |
| **Module ID** | RPT |
| **Platform** | Laravel API + Flutter |
| **Status** | In Progress |
| **Assigned Developer** | Arjun Patel |
| **Creation Date** | 2024-04-01 |
| **Last Updated** | 2024-06-15 |

---

## 2. Purpose & Scope

The Reports module enables users to generate, view, filter, and export data reports across various business metrics (revenue, user activity, task completion). Reports are generated asynchronously as background jobs and delivered via download link or in-app notification. Does NOT cover dashboard summary stats (see dashboard.md) or invoice generation (see payments.md).

---

## 3. User Roles & Permissions

| Role | Generate Reports | View Own Reports | View All Reports | Export Reports | Schedule Reports |
|---|---|---|---|---|---|
| Super Admin | ✅ | ✅ | ✅ | ✅ | ✅ |
| Admin | ✅ | ✅ | ✅ | ✅ | ✅ |
| Manager | ✅ | ✅ | ✅ (team) | ✅ | ❌ |
| User | ✅ | ✅ | ❌ | ✅ (own) | ❌ |

---

## 4. Feature List

- [DONE] Generate Revenue Report (by date range, monthly/quarterly/annual)
- [DONE] Generate User Activity Report
- [DONE] Generate Task Completion Report
- [DONE] Export as CSV
- [DONE] Export as XLSX (paginated for large datasets, fixes #201)
- [IN PROGRESS] Export as PDF
- [IN PROGRESS] Report scheduling (daily/weekly/monthly auto-generation)
- [PLANNED] Custom report builder (select columns, filters)
- [PLANNED] Report sharing (send to email)

---

## 5. API Endpoints

### POST /api/v1/reports/generate

| Field | Value |
|---|---|
| **Auth Required** | Yes |
| **Description** | Queues a report generation job. Returns a job ID to poll for status. |
| **Request Body** | `{ "type": "revenue|activity|tasks", "date_from": "YYYY-MM-DD", "date_to": "YYYY-MM-DD", "format": "csv|xlsx|pdf" }` |
| **Success (202)** | `{ "job_id": "uuid", "status": "queued", "message": "Report is being generated." }` |

### GET /api/v1/reports/status/{job_id}

| Field | Value |
|---|---|
| **Auth Required** | Yes |
| **Description** | Polls the status of a report generation job |
| **Success (200)** | `{ "job_id": "uuid", "status": "queued|processing|complete|failed", "download_url": "string|null" }` |

### GET /api/v1/reports

| Field | Value |
|---|---|
| **Auth Required** | Yes |
| **Description** | Lists all previously generated reports for the authenticated user |
| **Success (200)** | `{ "data": [ { "id": "uuid", "type": "string", "format": "string", "status": "string", "download_url": "string", "created_at": "ISO8601" } ] }` |

---

## 6. Database Tables

| Table | Key Columns | Notes |
|---|---|---|
| `reports` | id (UUID), user_id, type, format, status, file_path, created_at | Report job records |
| `jobs` | id, queue, payload, attempts, created_at | Laravel jobs queue |
| `failed_jobs` | id, uuid, payload, exception, failed_at | Failed job log |

---

## 7. Business Logic

### Report Generation Flow
1. User submits `POST /reports/generate` with type, date range, format.
2. Controller creates a `reports` record with `status = queued`.
3. `GenerateReportJob` is dispatched to the `reports` queue.
4. Job queries the relevant data, chunks the output (1000 rows per chunk for large datasets).
5. Writes output file to AWS S3 under `reports/{user_id}/{job_id}.{format}`.
6. Updates `reports` record: `status = complete`, `file_path = s3_url`.
7. Fires `ReportReady` event → Notifications module sends in-app + push notification.

**Large Dataset Handling (> 10,000 rows):**
- Export is chunked using Laravel's `chunk()` method.
- XLSX export uses OpenSpout for memory-efficient streaming.
- Max export size: 100,000 rows. Beyond this, user is prompted to narrow date range.

---

## 8. Mobile Screens (Flutter)

| Screen Name | Route | Data Source | Key Interactions |
|---|---|---|---|
| Reports List | `/reports` | GET /api/v1/reports | Tap to download, pull-to-refresh |
| Generate Report | `/reports/generate` | POST /api/v1/reports/generate | Select type, date range, format; submit |
| Report Status | `/reports/status` | GET /api/v1/reports/status/{id} | Auto-polling every 5s while status = processing |

---

## 9. Error Handling

| Error | HTTP Code | User-Facing Message |
|---|---|---|
| Invalid date range | 422 | "Date range cannot exceed 2 years." |
| Report > 100k rows | 422 | "Dataset too large. Please narrow your date range." |
| Job failed | 500 | "Report generation failed. Please try again." |
| Download URL expired | 403 | "Download link expired. Please regenerate the report." |

---

## 10. GitHub References

- Branch pattern: `feature/reports-[task]`, `fix/reports-[issue]`
- Related PRs: #95 (async generation), #110 (XLSX pagination fix)
- Open Issues: #201 (Large dataset crash — RESOLVED in v2.3.1)
- Milestone: v2.5.0 (PDF export, Report Scheduling)

---

## 11. Change Log

| Date | Developer | Change |
|---|---|---|
| 2024-06-15 | Arjun Patel | Added XLSX paginated export (fixes #201) |
| 2024-05-20 | Arjun Patel | Added CSV export, S3 file storage |
| 2024-04-01 | Arjun Patel | Initial module: async report generation, jobs queue |
