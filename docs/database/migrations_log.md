# Migrations Log | Product Management System

Chronological history of all database migrations applied to this project.

---

| # | Migration File                                               | Applied     | Description                                      |
|---|--------------------------------------------------------------|-------------|--------------------------------------------------|
| 1 | `0001_01_01_000000_create_users_table`                       | 2025-12-13  | Base users table (Laravel default)               |
| 2 | `0001_01_01_000001_create_cache_table`                       | 2025-12-13  | Cache and cache locks table                      |
| 3 | `0001_01_01_000002_create_jobs_table`                        | 2025-12-13  | Jobs, job batches, failed jobs tables            |
| 4 | `2025_12_13_035243_create_roles_and_permissions_tables`      | 2025-12-13  | Created `roles` and `permissions` tables         |
| 5 | `2025_12_13_035322_add_fields_to_users_table`                | 2025-12-13  | Added `role_id`, `phone`, `status`, `profile_photo_path`, location fields to `users` |
| 6 | `2025_12_13_093848_add_soft_deletes_to_users_and_roles_tables` | 2025-12-13 | Added `deleted_at` to `users` and `roles`       |
| 7 | `2025_12_16_131250_create_countries_table`                   | 2025-12-16  | Countries lookup table                           |
| 8 | `2025_12_16_131254_create_states_table`                      | 2025-12-16  | States lookup table (FK: countries)              |
| 9 | `2025_12_16_131330_create_cities_table`                      | 2025-12-16  | Cities lookup table (FK: states)                 |
| 10| `2025_12_16_131400_add_location_columns_to_users_table`      | 2025-12-16  | Added `country_id`, `state_id`, `city_id` to users |
| 11| `2025_12_16_163905_create_products_table`                    | 2025-12-16  | Products table (name, code, department, etc.)    |
| 12| `2025_12_16_165304_create_subscription_plans_table`          | 2025-12-16  | Subscription plans with pricing and phase flags  |
| 13| `2025_12_16_174415_create_product_modules_table`             | 2025-12-16  | Product-to-module pivot                          |
| 14| `2025_12_17_121636_create_submodules_table`                  | 2025-12-17  | Submodules (child of modules)                    |
| 15| `2025_12_17_125536_create_product_permissions_table`         | 2025-12-17  | Product-level permissions (child of submodules)  |
| 16| `2025_12_17_143647_create_customers_table`                   | 2025-12-17  | Customers table with full location and contact   |
| 17| `2025_12_19_194901_create_subscription_plan_modules_table`   | 2025-12-19  | Plan default module selections                   |
| 18| `2025_12_19_194902_create_subscription_plan_submodules_table`| 2025-12-19  | Plan default submodule selections                |
| 19| `2025_12_19_194903_create_subscription_plan_permission_table`| 2025-12-19  | Plan default permission selections               |
| 20| `2025_12_20_100000_create_customer_modules_table`            | 2025-12-20  | Customer-specific allowed modules snapshot       |
| 21| `2025_12_20_100001_create_customer_submodules_table`         | 2025-12-20  | Customer-specific allowed submodules snapshot    |
| 22| `2025_12_20_100002_create_customer_permission_table`         | 2025-12-20  | Customer-specific allowed permissions snapshot   |
| 23| `2025_12_20_100003_create_trn_customer_subscriptions_table`  | 2025-12-20  | Subscription header (one per customer+product)   |
| 24| `2025_12_20_100004_create_trn_customer_subscriptions_details_table` | 2025-12-20 | Subscription phase timeline rows       |
| 25| `2025_12_20_100005_create_customer_products_table`           | 2025-12-20  | Customer-product link with DB credentials        |
| 26| `2025_12_24_132851_create_product_users_table`               | 2025-12-24  | Device fingerprint registry                      |
| 27| `2025_12_24_143141_create_personal_access_tokens_table`      | 2025-12-24  | Laravel Sanctum API tokens                       |

---

## Notes

- All migrations that add columns to existing tables should have a `down()` method to revert.
- Run `php artisan migrate:status` to check which migrations have been applied.
- On fresh environments: `php artisan migrate --seed` (if seeders are added in future).
