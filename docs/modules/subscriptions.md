# Module: Subscription Management

---

## 1.0 Module Identity

| Field            | Details                            |
|------------------|------------------------------------|
| **Module Name**  | Subscription Management            |
| **Module Code**  | SUB                                |
| **Platform**     | Laravel + React (Inertia)          |
| **Status**       | Active                             |
| **Developer**    | *(Assign)*                         |
| **Created Date** | 2025-12-20                         |

---

## 2.0 Purpose & Scope

This is the **core transactional module** of the system. It handles creating, editing, and deactivating subscriptions that link a Customer to a Product via a chosen Subscription Plan. It generates sequential billing phase records (Trial → Free Service → Paid) and synchronizes the customer's allowed access rights (Modules/Submodules/Permissions) at the time of subscription creation. This module is **directly depended upon** by the License Verification API.

---

## 3.0 User Roles & Permissions

| Action                  | Admin | Manager |
|-------------------------|-------|---------|
| View Subscriptions List  | ✅    | ✅      |
| Create Subscription      | ✅    | ✅      |
| Edit Subscription        | ✅    | ✅      |
| Deactivate Subscription  | ✅    | ❌      |

---

## 4.0 Feature List

- [DONE] List all subscriptions with customer, product, and plan info
- [DONE] Create a subscription (generates phase detail records automatically)
- [DONE] Edit subscription (update plan, DB config, amounts, access rights)
- [DONE] Deactivate subscription (sets status = 0, blocks API access)
- [DONE] Dynamic plan loading (AJAX: load plans when a product is selected)
- [DONE] Dynamic plan details loading (auto-populate module/submodule/permission selections from plan)
- [DONE] Module / Submodule / Permission tree selection UI
- [DONE] DB credentials storage (host, name, user, password) per customer-product
- [PLANNED] Subscription Renewal flow
- [PLANNED] Payment receipt generation

---

## 5.0 API Endpoints (Web Routes)

| Method | Route                                     | Auth | Controller                        | Purpose                             |
|--------|-------------------------------------------|------|-----------------------------------|-------------------------------------|
| GET    | `/subscriptions`                          | ✅   | `SubscriptionController@index`    | List subscriptions                  |
| GET    | `/subscriptions/create`                   | ✅   | `SubscriptionController@create`   | Show create form                    |
| POST   | `/subscriptions`                          | ✅   | `SubscriptionController@store`    | Create subscription + phases        |
| GET    | `/subscriptions/{id}/edit`               | ✅   | `SubscriptionController@edit`     | Show edit form with current data    |
| PUT    | `/subscriptions/{id}`                    | ✅   | `SubscriptionController@update`   | Update subscription                 |
| DELETE | `/subscriptions/{id}`                    | ✅   | `SubscriptionController@destroy`  | Deactivate subscription             |
| GET    | `/api/products/{product}/plans`           | ✅   | `SubscriptionController@getPlans` | AJAX: get plans for a product       |
| GET    | `/api/plans/{plan}/details`              | ✅   | `SubscriptionController@getPlanDetails` | AJAX: get plan's access structure |
| GET    | `/api/products/{product}/structure`      | ✅   | `SubscriptionController@getProductStructure` | AJAX: full product hierarchy |

---

## 6.0 Database Tables

### `trn_customer_subscriptions` (Header)
| Column                  | Type      | Notes                                    |
|-------------------------|-----------|------------------------------------------|
| `id`                    | BIGINT PK |                                          |
| `customer_id`           | FK        | → `customers.id`                         |
| `product_id`            | FK        | → `products.id`                          |
| `subscription_plan_id`  | FK        | → `subscription_plans.id`                |
| `status`                | TINYINT   | 1 = Active, 0 = Inactive                 |
| `created_by`, `updated_by` | FK     | Audit trail                              |

### `trn_customer_subscriptions_details` (Phase Line Items)
| Column                     | Type         | Notes                                                |
|----------------------------|--------------|------------------------------------------------------|
| `id`                       | BIGINT PK    |                                                      |
| `customer_subscription_id` | FK           | → `trn_customer_subscriptions.id`                    |
| `subscription_type`        | TINYINT      | 1=Trial, 2=Free Service, 3=AMC, 4=Normal             |
| `transaction_no`           | VARCHAR      | Format: `SUB-{timestamp}-{rand4}`                    |
| `start_date`               | DATE         | Phase start                                          |
| `end_date`                 | DATE         | Phase end                                            |
| `receivable_amount`        | DECIMAL      | Amount to be received (0 for Trial/Free)             |
| `received_amount`          | DECIMAL      | Amount actually received                             |
| `payment_type`             | TINYINT      | Payment method (nullable for Trial/Free)             |
| `purchase_date`            | DATE         | Original purchase date                               |
| `status`                   | TINYINT      | 1 = Active                                           |

### `customer_products`
| Column                             | Notes                                         |
|------------------------------------|-----------------------------------------------|
| `customer_id`, `product_id`        | Composite unique key (one config per pair)    |
| `db_host`, `db_name`, `db_user`, `db_password` | Client's database credentials (API uses `db_name` as lookup) |
| `active_subscription_plan_id`      | FK → current active plan                      |
| `active_subscription_transaction_id`| FK → current active subscription header     |
| `is_trial_expire`, `is_free_service_expire`, `is_amc_expire` | Phase flag tracking |

### `customer_modules`, `customer_submodules`, `customer_permission`
Snapshot tables of allowed access rights per customer per product.

---

## 7.0 Business Logic

### Subscription Creation Flow
```
1. Validate: Check no other active subscription exists for this customer+product.
2. Begin DB Transaction.
3. Create TrnCustomerSubscription (header record, status=1).
4. Calculate Phase Timeline:
   a. If plan.is_trial AND trial_day > 0:
      → Create TRIAL detail (start=purchase_date, end=start+trial_days)
      → currentStart = trial_end
   b. If plan.is_free_service AND free_service_day > 0:
      → Create FREE_SERVICE detail
      → currentStart = free_service_end
   c. If plan.is_amc:
      → Create AMC detail (end = calculateTermEndDate(currentStart, plan.term))
   d. Else:
      → Create NORMAL detail (end = calculateTermEndDate(currentStart, plan.term))
5. Upsert CustomerProduct (stores DB credentials and links to new subscription).
6. syncAccessConfig() delete old customer permissions, insert new selection.
7. Commit Transaction.
```

### Term End Date Calculation
| Term Constant | Value | Adds |
|---------------|-------|------|
| `TERM_MONTHLY` | 1    | +1 Month |
| `TERM_YEARLY`  | 2    | +1 Year |
| `TERM_ONE_TIME`| 3    | +10 Years (hardcoded) |

### syncAccessConfig (Access Rights Sync)
```
1. DELETE all customer_modules WHERE customer_id + product_id
2. DELETE all customer_submodules WHERE customer_id + product_id
3. DELETE all customer_permission WHERE customer_id + product_id
4. FOR each selected module_id → INSERT customer_modules row (status=1)
5. FOR each selected submodule_id → lookup module_id → INSERT customer_submodules row
6. FOR each selected permission_id → lookup module_id + submodule_id → INSERT customer_permission row
```

> ⚠️ All 3 deletes + all inserts happen inside the same DB::transaction(). If any insert fails, all deletes are rolled back.

---

## 8.0 Mobile Screens

N/A — Admin panel only.

---

## 9.0 Error Handling

| Scenario                                      | Response                                           |
|-----------------------------------------------|----------------------------------------------------|
| Duplicate active subscription for product     | Redirect back with error on `product_id` field     |
| Invalid customer/product/plan IDs             | 422 Validation (exists rule)                       |
| DB transaction failure                        | Exception thrown, transaction rolled back          |

---

## 10.0 GitHub References

- **Branch Pattern**: `feature/sub-[task]`, `fix/sub-[issue]`
- **Controller**: `app/Http/Controllers/SubscriptionController.php`
- **Routes**: `routes/web.php` lines 49–55

---

## 11.0 Change Log

| Date       | Developer | Change                                                   |
|------------|-----------|----------------------------------------------------------|
| 2025-12-20 | —         | Initial subscription CRUD with phase timeline engine     |
| 2025-12-20 | —         | syncAccessConfig customer permissions snapshot system  |
| 2025-12-24 | —         | Edit mode: load existing selections for modules/permissions |
