# Module: Reports

---

## 1.0 Module Identity

| Field            | Details                              |
|------------------|--------------------------------------|
| **Module Name**  | Reports                              |
| **Module Code**  | RPT                                  |
| **Platform**     | Laravel + React (Inertia)            |
| **Status**       | Active                               |
| **Developer**    | *(Assign)*                           |
| **Created Date** | 2025-12-24                           |

---

## 2.0 Purpose & Scope

Provides operational reporting for internal admin users. Currently includes two report views: (1) **User Report** shows all registered devices (product_users) with filtering by product and customer, and (2) **Product Customer Report** shows all customer-product subscription relationships with drill-down details including device list and subscription history. This module does **not** generate PDF/Excel exports (planned).

---

## 3.0 User Roles & Permissions

| Action               | Admin | Manager | QA |
|----------------------|-------|---------|-----|
| View User Report      | ✅    | ✅      | ✅  |
| Toggle Device Status  | ✅    | ❌      | ❌  |
| View Product Customer Report | ✅ | ✅   | ✅  |
| View Detail Modal     | ✅    | ✅      | ✅  |

---

## 4.0 Feature List

- [DONE] User Report list all registered devices (product_users), filter by product + customer, sort by name/email/customer/product/status
- [DONE] Toggle device active/inactive status from User Report
- [DONE] Product Customer Report list all customer subscriptions with device count + plan info
- [DONE] Product Customer Report Detail (AJAX modal) shows device list + subscription history + current subscription config
- [PLANNED] Export to Excel/CSV
- [PLANNED] Date range filtering
- [PLANNED] Subscription expiry alert report

---

## 5.0 API Endpoints

| Method | Route                                              | Auth | Controller                              | Purpose                           |
|--------|----------------------------------------------------|------|-----------------------------------------|-----------------------------------|
| GET    | `/reports/user-report`                             | ✅   | `UserReportController@index`            | User/Device report (paginated)    |
| POST   | `/reports/user-report/{productUser}/toggle-status` | ✅   | `UserReportController@toggleStatus`     | Enable/disable a device           |
| GET    | `/reports/product-customer-report`                 | ✅   | `ProductCustomerReportController@index` | Customer-product subscriptions    |
| GET    | `/reports/product-customer-report/details`         | ✅   | `ProductCustomerReportController@details` | AJAX: drill-down details        |

---

### GET `/reports/product-customer-report/details`

| Field         | Value                                   |
|---------------|-----------------------------------------|
| **Auth Required** | Yes (session)                       |
| **Request Params** | `customer_id` (required), `product_id` (required) |
| **Response**  | JSON with `customer`, `product_users`, `subscription_history`, `current_subscription` |

---

## 6.0 Database Tables

| Table                              | Used For                                   |
|------------------------------------|--------------------------------------------|
| `product_users`                    | Device registry User Report source       |
| `customer_products`                | Customer-product links PC Report source  |
| `trn_customer_subscriptions_details`| Subscription history in detail modal      |
| `subscription_plans`               | Plan name + price in reports               |
| `customers`                        | Customer name/company in both reports      |
| `products`                         | Product filter dropdown                    |

---

## 7.0 Business Logic

### User Report
1. Loads `product_users` with eager-loaded `product`, `customer`, `creator`, `updater`.
2. Filters by `product_id` and/or `customer_id` if passed as query params.
3. Customer dropdown is dynamically filtered based on selected product (only customers who have a `customer_products` record for that product).
4. Sorting uses `join` clauses for relational columns (customer name, product name).

### Product Customer Report
1. Loads `customer_products` with related plan and subscription.
2. Adds a subquery to count registered devices per customer-product pair.
3. Supports filter by product and search by customer name/email.
4. Detail modal fires `GET /reports/product-customer-report/details?customer_id=X&product_id=Y` and renders:
   - Customer info
   - All devices registered (from `product_users`)
   - Full subscription timeline (from `trn_customer_subscriptions_details`)
   - Current active plan config (from `customer_products`)

---

## 9.0 Error Handling

| Scenario                | Response                          |
|-------------------------|-----------------------------------|
| Invalid customer_id/product_id in details | 422 Validation error |
| No data found           | Empty collections returned (no 404) |

---

## 10.0 GitHub References

- **Branch Pattern**: `feature/rpt-[task]`, `fix/rpt-[issue]`
- **Controllers**: `UserReportController.php`, `ProductCustomerReportController.php`
- **Routes**: `routes/web.php` lines 63–67

---

## 11.0 Change Log

| Date       | Developer | Change                                           |
|------------|-----------|--------------------------------------------------|
| 2025-12-24 | —         | User Report + Product Customer Report created    |
| 2025-12-24 | —         | Device toggle-status from User Report added      |
