# Migrations Log — AXN-042 Axone Client Portal

All database migrations in chronological order. Run `php artisan migrate:status` to verify current state.

---

| Migration File | Date | Developer | Description |
|---|---|---|---|
| `2024_01_15_000001_create_users_table.php` | 2024-01-15 | Rahul Mehta | Initial users table with soft deletes |
| `2024_01_15_000002_create_password_reset_tokens_table.php` | 2024-01-15 | Rahul Mehta | Password reset tokens |
| `2024_01_15_000003_create_personal_access_tokens_table.php` | 2024-01-15 | Rahul Mehta | Sanctum tokens table |
| `2024_01_20_000001_create_oauth_providers_table.php` | 2024-01-20 | Priya Sharma | Google OAuth provider links |
| `2024_02_10_000001_create_activity_logs_table.php` | 2024-02-10 | Priya Sharma | User activity logging |
| `2024_03_01_000001_create_roles_and_permissions_tables.php` | 2024-03-01 | Rahul Mehta | spatie/permission tables |
| `2024_04_01_000001_create_reports_table.php` | 2024-04-01 | Arjun Patel | Reports with UUID PK and S3 path |
| `2024_04_01_000002_create_jobs_table.php` | 2024-04-01 | Arjun Patel | Laravel queue jobs table |
| `2024_04_01_000003_create_failed_jobs_table.php` | 2024-04-01 | Arjun Patel | Failed jobs log |
| `2024_06_01_000001_create_notifications_table.php` | 2024-06-01 | Sneha Joshi | Laravel built-in notifications |
| `2024_06_01_000002_create_user_fcm_tokens_table.php` | 2024-06-01 | Sneha Joshi | Firebase push notification tokens |
| `2024_06_20_000001_add_last_login_at_to_users_table.php` | 2024-06-20 | Rahul Mehta | Added last_login_at column to users |

---

## Notes

- All migrations have a working `down()` method for rollback.
- Do NOT rollback migrations on production without Tech Lead approval.
- If adding a column to an existing table, always make it nullable or provide a default to avoid locking large tables.
- Migration naming convention: `YYYY_MM_DD_NNNNNN_description.php`
