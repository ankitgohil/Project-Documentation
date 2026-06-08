# Project Overview | Product Management System

---

## 1. Project Identity1

| Field              | Details                                          |
|--------------------|--------------------------------------------------|
| **Project Name**   | Product Management System (License Server)       |
| **Project Code**   | AXN-PM-01                                        |
| **Client Name**    | Axone Infotech (Internal)                        |
| **Platform**       | Laravel 12 (Backend) + React 18 + Inertia.js     |
| **Start Date**     | December 2025                                    |
| **Live URL**       | *(Set in .env: APP_URL)*                         |
| **Repo**           | Axone-Infotech/Product-management                |

---

## 2. Tech Stack

| Layer                | Technology                                         |
|----------------------|----------------------------------------------------|
| **Language**         | PHP 8.2+                                           |
| **Backend Framework**| Laravel 12.0                                       |
| **Frontend**         | React 18.2 + Inertia.js 2.0                        |
| **Styling**          | Tailwind CSS 3.2.1 + Headless UI 2.0               |
| **Build Tool**       | Vite 7 (via laravel-vite-plugin)                   |
| **Database**         | MySQL / MariaDB                                    |
| **Auth System**      | Laravel Breeze (Session-based, Sanctum for API)    |
| **Icons**            | Heroicons React 2.2                                |
| **API Client**       | Axios 1.11                                         |
| **Utilities**        | Lodash 4.17, use-debounce 10.0                     |
| **HTTP Routing**     | Ziggy 2.0 (shares Laravel routes with React)       |
| **Font**             | Roboto (via Tailwind config)                       |

---

## 3. Team Members

| Name          | Role           | GitHub Username | Contact           |
|---------------|----------------|-----------------|-------------------|
| *(Tech Lead)* | Tech Lead / PM | *(Add Here)*    | *(Add Email)*     |
| *(Dev 1)*     | Full Stack Dev  | *(Add Here)*    | *(Add Email)*     |

---

## 4. Module List

| Module Name              | Status      | Lead Developer  | Last Updated | Doc File                                   |
|--------------------------|-------------|-----------------|-------------|---------------------------------------------|
| Authentication           | Active      | —               | 2025-12-24  | [auth.md](./modules/auth.md)                |
| Dashboard                | Active      | —               | 2025-12-24  | [dashboard.md](./modules/dashboard.md)      |
| User Management          | Active      | —               | 2025-12-24  | [users.md](./modules/users.md)              |
| Role Management          | Active      | —               | 2025-12-13  | [roles.md](./modules/roles.md)              |
| Product Catalog          | Active      | —               | 2025-12-16  | [products.md](./modules/products.md)        |
| Modules & Submodules     | Active      | —               | 2025-12-17  | [modules.md](./modules/modules.md)          |
| Permissions              | Active      | —               | 2025-12-17  | [permissions.md](./modules/permissions.md)  |
| Subscription Plans       | Active      | —               | 2025-12-16  | [plans.md](./modules/plans.md)              |
| Customer Management      | Active      | —               | 2025-12-17  | [customers.md](./modules/customers.md)      |
| Subscription Management  | Active      | —               | 2025-12-20  | [subscriptions.md](./modules/subscriptions.md) |
| Location Masters         | Active      | —               | 2025-12-16  | [locations.md](./modules/locations.md)      |
| Reports                  | Active      | —               | 2025-12-24  | [reports.md](./modules/reports.md)          |
| License Verification API | Active      | —               | 2025-12-24  | [license_api.md](./modules/license_api.md)  |

---

## 5. Environment Details

| Environment | URL                          | Server Type         |
|-------------|------------------------------|---------------------|
| Local (Dev) | `http://localhost/Product-management/public` | WAMP64 (Windows)    |
| Staging     | *(Configure in .env)*        | Ubuntu + Nginx/Apache |
| Production  | *(Configure in .env)*        | Ubuntu + PHP-FPM    |

> **Note:** No credentials are stored here. See [ENV_VARIABLES.md](./setup/ENV_VARIABLES.md).

---

## 6. Key Business Rules

1. **One Active Subscription Per Customer Per Product** A customer can only have one active (`status = 1`) subscription for any given product at a time. Attempting to create a second one is blocked by the system.
2. **Hierarchical Subscription Phases** A subscription is composed of sequential phases: Trial → Free Service → AMC/Normal Paid. Each phase has its own start and end date, stored as separate rows in `trn_customer_subscriptions_details`.
3. **Access Right Snapshots** When a subscription is created, the allowed Modules, Submodules, and Permissions are physically copied into customer-specific tables (`customer_modules`, `customer_submodules`, `customer_permission`). The API reads from these, not directly from the product definition.
4. **Device Fingerprint-Based Locking** End-user devices are registered by their hardware fingerprint (UUID + CPU ID + Motherboard SN + Disk SN). An individual device can be disabled without affecting the whole customer account.
5. **DB Name as Customer Identifier** The external API identifies a customer's product configuration using a unique `db_name` stored in the `customer_products` table.

---

## 7. External Integrations

| Integration      | Purpose             | Status    |
|------------------|---------------------|-----------|
| None             | —                   | —         |
| *(SMS Gateway)*  | *(Planned)*         | Planned   |
| *(Email)*        | Laravel Mail (SMTP) | Active    |

---

## 8. Known Issues / Technical Debt

| # | Issue                                              | Severity | Area                  |
|---|---------------------------------------------------|----------|-----------------------|
| 1 | API endpoint has no API key authentication         | High     | License API           |
| 2 | No response caching on identify-customer endpoint | Medium   | Performance           |
| 3 | `syncAccessConfig` uses N+1 queries inside loops  | Medium   | Subscription Store    |
| 4 | Transaction number generated with `time() + rand` (not guaranteed unique) | Low | Subscription Details |
| 5 | One-time plan defaults to +10 years (hardcoded)   | Low      | Business Logic        |

---

## 9. Related Documents

| Document               | Path                                            |
|------------------------|-------------------------------------------------|
| API Documentation      | [api/endpoints.md](./api/endpoints.md)          |
| Database Schema        | [database/schema.md](./database/schema.md)      |
| Migration Log          | [database/migrations_log.md](./database/migrations_log.md) |
| Local Setup Guide      | [setup/LOCAL_SETUP.md](./setup/LOCAL_SETUP.md)  |
| ENV Variables          | [setup/ENV_VARIABLES.md](./setup/ENV_VARIABLES.md) |
| Branch Strategy        | [github/BRANCH_STRATEGY.md](./github/BRANCH_STRATEGY.md) |
| PR Template            | [github/PR_TEMPLATE.md](./github/PR_TEMPLATE.md)|
| Developer Guide        | [DEVELOPER_GUIDE.md](./DEVELOPER_GUIDE.md)      |
| Changelog              | [CHANGELOG.md](./CHANGELOG.md)                  |