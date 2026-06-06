# Changelog | Product Management System

All notable changes to this project are documented here in reverse chronological order.

---

## [v1.0.0] | 2025-12-24 | Ankit Gohil

### Added
- **License Verification API** `POST /api/identify-customer` for external device authentication and hierarchical access rights delivery.
- **Product Users (Device Tracking)** `product_users` table to register and track hardware-fingerprinted devices.
- **Personal Access Tokens** Added `personal_access_tokens` table (Sanctum).

---

## [v0.9.0] | 2025-12-20 | Ankit Gohil

### Added
- **Subscription Management Module** Full CRUD for assigning customers to subscription plans.
- **Subscription Timeline Engine** Auto-generates Trial → Free Service → AMC/Normal date chain from plan configuration.
- **Customer Access Rights Sync** On subscription create/update, the system syncs allowed modules/submodules/permissions into customer-specific tables.
- **Customer Products Table** Links customer to product with DB credentials and active plan tracking.
- **Subscription Details Table** Line-level subscription phase records (trn_customer_subscriptions_details).

---

## [v0.8.0] | 2025-12-19 | Ankit Gohil

### Added
- **Subscription Plan Mapping** Pivot tables to associate Plans with Modules, Submodules, and Permissions.

---

## [v0.7.0] | 2025-12-17 | Ankit Gohil

### Added
- **Customer Management Module** Full CRUD for customers with status toggle and location linkage.
- **Submodules Module** CRUD + bulk status update + per-product module relationships.
- **Product Permissions Module** CRUD for granular permissions under submodules.

---

## [v0.6.0] | 2025-12-16 | Ankit Gohil

### Added
- **Product Catalog Module** Full CRUD for products with department classification.
- **Subscription Plans Module** Full CRUD with term/trial/AMC configuration.
- **Location Masters** Countries, States, Cities tables and controllers.
- **Location Columns on Users** Added `country_id`, `state_id`, `city_id` to `users`.

---

## [v0.5.0] | 2025-12-13 | Ankit Gohil

### Added
- **Roles & Permissions Tables** `roles` and `permissions` tables with pivot.
- **Extended User Fields** Added `role_id`, `phone`, `status`, `profile_photo_path` to `users`.
- **Soft Deletes** Added `deleted_at` to `users` and `roles`.

---

## [v0.1.0] | 2025-12-13 (Initial Setup) | Ankit Gohil

### Added
- Laravel 12 project scaffold.
- Breeze authentication (React + Inertia stack).
- Base users, cache, and jobs tables.
- Vite + Tailwind CSS configuration.
