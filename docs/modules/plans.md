# Module: Subscription Plans

---

## 1.0 Module Identity

| Field            | Details                            |
|------------------|------------------------------------|
| **Module Name**  | Subscription Plans                 |
| **Module Code**  | PLAN                               |
| **Platform**     | Laravel + React (Inertia)          |
| **Status**       | Active                             |
| **Developer**    | *(Assign)*                         |
| **Created Date** | 2025-12-16                         |

---

## 2.0 Purpose & Scope

Manages the pricing templates (plans) that are linked to each product. Each plan configures pricing, billing term (Monthly/Yearly/One-Time), and optional phases (Trial days, Free Service days, AMC flag). When a subscription is created, the system reads this plan configuration to generate the multi-phase billing timeline. This module does **not** directly assign plans to customers that happens in the Subscription module.

---

## 3.0 User Roles & Permissions

| Action       | Admin | Manager |
|--------------|-------|---------|
| View Plans    | ✅    | ✅      |
| Create Plan   | ✅    | ❌      |
| Edit Plan     | ✅    | ❌      |
| Delete Plan   | ✅    | ❌      |

---

## 4.0 Feature List

- [DONE] List plans with product name, pricing, and status
- [DONE] Create plan with term (Monthly/Yearly/One-Time), price, sale_price, AMC price
- [DONE] Configure Trial flag + trial days
- [DONE] Configure Free Service flag + free service days
- [DONE] Configure AMC flag
- [DONE] Edit plan
- [DONE] Delete plan

---

## 5.0 API Endpoints

| Method | Route                      | Auth | Controller                        | Purpose              |
|--------|----------------------------|------|-----------------------------------|----------------------|
| GET    | `/subscription-plans`       | ✅   | `SubscriptionPlanController@index` | List plans           |
| POST   | `/subscription-plans`       | ✅   | `SubscriptionPlanController@store` | Create plan          |
| PUT    | `/subscription-plans/{id}`  | ✅   | `SubscriptionPlanController@update`| Update plan          |
| DELETE | `/subscription-plans/{id}`  | ✅   | `SubscriptionPlanController@destroy`| Delete plan         |

---

## 6.0 Database Tables

### `subscription_plans`
| Column              | Type         | Notes                                               |
|---------------------|--------------|-----------------------------------------------------|
| `product_id`        | FK           | → `products.id`                                     |
| `name`              | VARCHAR      | Plan display name                                   |
| `code`              | VARCHAR      | Unique code per plan                                |
| `term`              | TINYINT      | 1=Monthly, 2=Yearly, 3=One-Time                     |
| `price`             | DECIMAL      | List price                                          |
| `sale_price`        | DECIMAL      | Actual sale price used in billing                   |
| `amc_price`         | DECIMAL      | Annual maintenance price (if is_amc=true)           |
| `is_trial`          | BOOLEAN      | Enable trial phase                                  |
| `trial_day`         | INTEGER      | Number of trial days                                |
| `is_free_service`   | BOOLEAN      | Enable free service phase                           |
| `free_service_day`  | INTEGER      | Number of free service days                         |
| `is_amc`            | BOOLEAN      | Whether this is an AMC-type plan                    |
| `status`            | TINYINT      | 1=Active, 0=Inactive                                |

**Pivot Tables (Plan Access Defaults):**
- `subscription_plan_modules` default modules included in this plan
- `subscription_plan_submodules` default submodules
- `subscription_plan_permission` default permissions

---

## 7.0 Business Logic

1. A plan belongs to **one product** only.
2. The `term` determines how `calculateTermEndDate()` works in `SubscriptionController`.
3. `is_trial + trial_day` controls whether a Trial phase row is generated in `trn_customer_subscriptions_details`.
4. `is_free_service + free_service_day` controls whether a Free Service row is generated.
5. `is_amc` controls whether the paid phase is labeled AMC or Normal.
6. Modules/Submodules/Permissions assigned to a plan are **suggestions** admins can override them during subscription creation.

---

## 9.0 Error Handling

| Scenario           | Response                       |
|--------------------|--------------------------------|
| Duplicate plan code| 422 unique validation error    |
| Invalid product_id | 422 `exists` validation        |

---

## 10.0 GitHub References

- **Branch Pattern**: `feature/plan-[task]`
- **Controller**: `app/Http/Controllers/SubscriptionPlanController.php`

---

## 11.0 Change Log

| Date       | Developer | Change                                                  |
|------------|-----------|----------------------------------------------------------|
| 2025-12-16 | —         | Subscription plans table and CRUD created               |
| 2025-12-19 | —         | Plan-to-module/submodule/permission pivot tables added  |
