# PROJECT OVERVIEW — AXN-042 | Axone Client Portal

| Field | Value |
|---|---|
| **Project Name** | Axone Client Portal |
| **Project Code** | AXN-042 |
| **Client Name** | Axone Infotech (Internal) |
| **Platform** | Laravel (Backend) + Flutter (Mobile) |
| **Start Date** | 2024-01-15 |
| **Live URL** | https://portal.axoneinfotech.com |
| **Version** | v2.4.0 |
| **Last Updated** | 2024-06-20 |

---

## 1. Tech Stack

| Layer | Technology | Version |
|---|---|---|
| Backend Language | PHP | 8.2 |
| Framework | Laravel | 11.x |
| Database | MySQL | 8.0 |
| Mobile | Flutter | 3.19 |
| Cache | Redis | 7.x |
| Queue | Laravel Horizon | — |
| Storage | AWS S3 | — |
| Auth | Laravel Sanctum (JWT) | — |
| Packages | spatie/permission, barryvdh/debugbar, league/flysystem-aws-s3-v3 | — |

---

## 2. Team Members

| Name | Role | GitHub | Contact |
|---|---|---|---|
| Rahul Mehta | Tech Lead | @rahulmehta-axn | rahul@axoneinfotech.com |
| Priya Sharma | Senior Developer | @priyasharma-axn | priya@axoneinfotech.com |
| Arjun Patel | Developer | @arjunpatel-axn | arjun@axoneinfotech.com |
| Sneha Joshi | Junior Developer | @snehajoshi-axn | sneha@axoneinfotech.com |
| Karan Desai | QA Engineer | @karandesai-axn | karan@axoneinfotech.com |
| Neeha Kulkarni | Project Manager | @neehakulkarni-axn | neeha@axoneinfotech.com |

---

## 3. Module List

| Module Name | Status | Lead Developer | Last Updated | Doc File |
|---|---|---|---|---|
| **Authentication** | Active | Priya Sharma | 2024-06-01 | [auth.md](modules/auth.md) |
| **User Dashboard** | Active | Priya Sharma | 2024-06-10 | [dashboard.md](modules/dashboard.md) |
| **Reports Module** | In Progress | Arjun Patel | 2024-06-15 | [reports.md](modules/reports.md) |
| **Admin Panel** | Active | Rahul Mehta | 2024-05-20 | [admin.md](modules/admin.md) |
| **Notifications** | In Progress | Sneha Joshi | 2024-06-18 | [notifications.md](modules/notifications.md) |
| **Payment Gateway** | Planned | TBD | — | [payments.md](modules/payments.md) |

---

## 4. Environment Details

| Environment | URL | Server |
|---|---|---|
| Local | http://localhost:8000 | Local (Herd/Valet) |
| Staging | https://staging-portal.axoneinfotech.com | AWS EC2 t3.medium |
| Production | https://portal.axoneinfotech.com | AWS EC2 t3.large |

> ⚠️ No credentials stored here. See ENV_VARIABLES.md and Bitwarden vault.

---

## 5. Key Business Rules

1. **Role Hierarchy**: Super Admin > Admin > Manager > User > Guest. Each role has distinct permission sets managed via spatie/permission.
2. **Token Expiry**: JWT access tokens expire in 60 minutes; refresh tokens valid for 30 days.
3. **Report Generation**: All reports are generated as background jobs via Laravel Queues. Real-time updates via WebSockets (Pusher).
4. **Soft Deletes**: All user and data records use Laravel soft deletes — never hard delete in production.
5. **File Uploads**: Maximum file size is 10MB. Accepted types: PDF, DOCX, XLSX, PNG, JPG. Stored in AWS S3 bucket `axn-portal-uploads`.

---

## 6. External Integrations

| Service | Purpose | Provider |
|---|---|---|
| Payment Gateway | Invoice payments | Razorpay |
| SMS | OTP delivery | Twilio |
| Email | Transactional email | Mailgun |
| Push Notifications | Mobile alerts | Firebase FCM |
| File Storage | Document/media storage | AWS S3 |
| Maps | Client location display | Google Maps SDK |

---

## 7. Known Issues

| Issue | Severity | Raised By | GitHub Issue |
|---|---|---|---|
| Expired JWT token does not redirect to login on mobile | High | Karan Desai | #187 |
| Reports export fails for datasets > 50,000 rows | Medium | Arjun Patel | #201 |
| Push notification delivery delay on iOS (> 5 min) | Low | Sneha Joshi | #215 |

---

## 8. Related Documents

- [API Endpoints](api/endpoints.md)
- [Database Schema](database/schema.md)
- [Local Setup Guide](setup/LOCAL_SETUP.md)
- [Branch Strategy](github/BRANCH_STRATEGY.md)
- [Developer Guide](DEVELOPER_GUIDE.md)
- [GitHub Project Board](https://github.com/orgs/axoneinfotech/projects/42)
