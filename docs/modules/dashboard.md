# Module: Dashboard

---

## 1.0 Module Identity

| Field            | Details                            |
|------------------|------------------------------------|
| **Module Name**  | Dashboard                          |
| **Module Code**  | DASH                               |
| **Platform**     | Laravel + React (Inertia)          |
| **Status**       | Active                             |
| **Developer**    | *(Assign)*                         |
| **Created Date** | 2025-12-13                         |

---

## 2.0 Purpose & Scope

The Dashboard is the main landing page for authenticated admin users. It provides a summary overview of key metrics (total customers, active subscriptions, products, expiring subscriptions) and acts as the navigation hub for the system. It does **not** contain business transaction logic.

---

## 3.0 Feature List

- [DONE] Summary statistics cards (total customers, active subscriptions, total products, expiring soon)
- [DONE] Quick navigation links to all major modules
- [DONE] Responsive layout with sidebar navigation
- [PLANNED] Revenue chart (monthly billing summary)
- [PLANNED] Recent activity feed

---

## 5.0 API Endpoints

| Method | Route        | Auth | Controller | Purpose          |
|--------|--------------|------|------------|------------------|
| GET    | `/dashboard` | ✅   | Closure    | Render dashboard |

---

## 6.0 Database Tables

Dashboard reads aggregate counts from: `customers`, `products`, `trn_customer_subscriptions`, `trn_customer_subscriptions_details`.

---

## 11.0 Change Log

| Date       | Developer | Change                           |
|------------|-----------|----------------------------------|
| 2025-12-13 | —         | Initial dashboard stub created   |
