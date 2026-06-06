# Database Schema | Product Management System

---

## ERD Overview (Text Format)

```
customers
  ├── country_id → countries.id
  ├── state_id   → states.id
  ├── city_id    → cities.id
  └── has many → customer_products
                    ├── product_id → products.id
                    ├── active_subscription_plan_id → subscription_plans.id
                    └── active_subscription_transaction_id → trn_customer_subscriptions.id

products
  ├── has many → product_modules
  │                └── module_id → modules.id
  │                     └── has many → submodules
  │                                       └── has many → product_permissions
  ├── has many → subscription_plans
  └── has many → product_users

subscription_plans
  ├── product_id → products.id
  ├── has many → subscription_plan_modules
  ├── has many → subscription_plan_submodules
  └── has many → subscription_plan_permission

trn_customer_subscriptions (Header)
  ├── customer_id → customers.id
  ├── product_id  → products.id
  ├── subscription_plan_id → subscription_plans.id
  └── has many → trn_customer_subscriptions_details (Phases)

customer_modules
  ├── customer_id → customers.id
  ├── product_id  → products.id
  └── module_id   → modules.id

customer_submodules
  ├── customer_id  → customers.id
  ├── product_id   → products.id
  ├── module_id    → modules.id
  └── submodule_id → submodules.id

customer_permission
  ├── customer_id   → customers.id
  ├── product_id    → products.id
  ├── module_id     → modules.id
  ├── submodule_id  → submodules.id
  └── permission_id → product_permissions.id

product_users
  ├── customer_id → customers.id
  └── product_id  → products.id
```

---

## Table Definitions

### `users`
| Column                | Type         | Nullable | Notes                      |
|-----------------------|--------------|----------|----------------------------|
| `id`                  | BIGINT PK    |          |                            |
| `name`                | VARCHAR(255) |          |                            |
| `email`               | VARCHAR(255) |          | Unique                     |
| `password`            | VARCHAR(255) |          | Bcrypt                     |
| `role_id`             | BIGINT FK    | ✅       | → `roles.id`               |
| `phone`               | VARCHAR(15)  | ✅       |                            |
| `status`              | TINYINT      |          | 1=Active, 0=Inactive        |
| `profile_photo_path`  | VARCHAR      | ✅       |                            |
| `country_id`, `state_id`, `city_id` | BIGINT FK | ✅ | Location        |
| `email_verified_at`   | TIMESTAMP    | ✅       |                            |
| `deleted_at`          | TIMESTAMP    | ✅       | Soft delete                |
| `created_at`, `updated_at` | TIMESTAMP |       |                            |

---

### `customers`
| Column          | Type         | Nullable | Notes             |
|-----------------|--------------|----------|-------------------|
| `id`            | BIGINT PK    |          |                   |
| `company_name`  | VARCHAR(100) |          |                   |
| `first_name`    | VARCHAR(100) |          |                   |
| `last_name`     | VARCHAR(100) |          |                   |
| `email`         | VARCHAR(100) |          | Unique            |
| `phone`         | VARCHAR(13)  |          |                   |
| `address_line1` | VARCHAR(150) |          |                   |
| `address_line2` | VARCHAR(150) | ✅       |                   |
| `country_id`    | BIGINT FK    |          | → `countries.id`  |
| `state_id`      | BIGINT FK    |          | → `states.id`     |
| `city_id`       | BIGINT FK    |          | → `cities.id`     |
| `postal_code`   | INTEGER      |          |                   |
| `status`        | TINYINT      |          | 1=Active, 0=Inactive |
| `remark`        | VARCHAR(255) | ✅       |                   |
| `created_by`, `updated_by` | BIGINT FK | ✅ | Audit        |
| `created_ip`, `updated_ip` | VARCHAR   | ✅ | Audit        |
| `created_source`, `updated_source` | VARCHAR | ✅ |         |

---

### `products`
| Column        | Type         | Notes                                    |
|---------------|--------------|------------------------------------------|
| `name`        | VARCHAR(100) |                                          |
| `code`        | VARCHAR(50)  | Unique short code                        |
| `department`  | TINYINT      | 1=Laravel, 2=Desktop, 3=MVC, 4=Mobile   |
| `database_type`| VARCHAR(50) | e.g. MySQL, MSSQL                        |
| `language`    | VARCHAR(50)  | e.g. PHP, C#, Dart                       |
| `description` | TEXT         | ✅                                       |
| `status`      | TINYINT      | 1=Active, 0=Inactive                     |

---

### `subscription_plans`
| Column             | Type     | Notes                                     |
|--------------------|----------|-------------------------------------------|
| `product_id`       | FK       | → `products.id`                           |
| `name`             | VARCHAR  |                                           |
| `code`             | VARCHAR  | Unique per plan                           |
| `term`             | TINYINT  | 1=Monthly, 2=Yearly, 3=One-Time           |
| `price`            | DECIMAL  | List price                                |
| `sale_price`       | DECIMAL  | Used in billing                           |
| `amc_price`        | DECIMAL  | AMC rate                                  |
| `is_trial`         | BOOLEAN  |                                           |
| `trial_day`        | INTEGER  |                                           |
| `is_free_service`  | BOOLEAN  |                                           |
| `free_service_day` | INTEGER  |                                           |
| `is_amc`           | BOOLEAN  |                                           |
| `status`           | TINYINT  |                                           |

---

### `trn_customer_subscriptions`
| Column                  | Type    | Notes                        |
|-------------------------|---------|------------------------------|
| `customer_id`           | FK      | → `customers.id`             |
| `product_id`            | FK      | → `products.id`              |
| `subscription_plan_id`  | FK      | → `subscription_plans.id`    |
| `status`                | TINYINT | 1=Active, 0=Deactivated       |

---

### `trn_customer_subscriptions_details`
| Column                     | Type         | Notes                                      |
|----------------------------|--------------|--------------------------------------------|
| `customer_subscription_id` | FK           | → `trn_customer_subscriptions.id`          |
| `subscription_type`        | TINYINT      | 1=Trial, 2=Free Service, 3=AMC, 4=Normal  |
| `transaction_no`           | VARCHAR(50)  | `SUB-{time}-{rand4}`                       |
| `start_date`               | DATE         |                                            |
| `end_date`                 | DATE         |                                            |
| `receivable_amount`        | DECIMAL(10,2)|                                            |
| `received_amount`          | DECIMAL(10,2)|                                            |
| `payment_type`             | TINYINT      | ✅ nullable for Trial/Free                 |
| `purchase_date`            | DATE         |                                            |

---

### `customer_products`
| Column                             | Type    | Notes                                          |
|------------------------------------|---------|------------------------------------------------|
| `customer_id`                      | FK      | → `customers.id`                               |
| `product_id`                       | FK      | → `products.id`                                |
| `active_subscription_plan_id`      | FK      | → `subscription_plans.id`                      |
| `active_subscription_transaction_id`| FK     | → `trn_customer_subscriptions.id`              |
| `db_host`                          | VARCHAR | Client's DB server host                        |
| `db_name`                          | VARCHAR | **Used as API lookup key**                     |
| `db_user`                          | VARCHAR | Client DB username                             |
| `db_password`                      | VARCHAR | Client DB password (stored plain — risk ⚠️)   |
| `is_trial_expire`, `is_free_service_expire`, `is_amc_expire` | TINYINT | Phase flags |
| `status`                           | TINYINT | 1=Active                                       |

---

### `product_users`
| Column                       | Type         | Notes                                   |
|------------------------------|--------------|-----------------------------------------|
| `customer_id`                | FK           | → `customers.id`                        |
| `product_id`                 | FK           | → `products.id`                         |
| `first_name`, `last_name`    | VARCHAR      |                                         |
| `email`                      | VARCHAR      |                                         |
| `phone`                      | VARCHAR      |                                         |
| `device_uuid`                | VARCHAR      | Hardware UUID                           |
| `cpu_processor_id`           | VARCHAR      |                                         |
| `motherboard_serial_number`  | VARCHAR      |                                         |
| `primary_disk_serial_number` | VARCHAR      |                                         |
| `device_fingerprint`         | VARCHAR      | **Primary lookup key for API**          |
| `public_ip`, `local_ip`      | VARCHAR      | Updated on each API call                |
| `status`                     | TINYINT      | 1=Active, 0=Disabled                    |
| `created_source`, `updated_source` | VARCHAR| 'API' or 'WEB'                         |

---

## Key Business Indexes (Recommended)

| Table                              | Suggested Index                              |
|------------------------------------|----------------------------------------------|
| `customer_products`                | `db_name` (unique) — API lookup              |
| `product_users`                    | `device_fingerprint` + `customer_id` + `product_id` |
| `trn_customer_subscriptions`       | `customer_id` + `product_id` + `status`      |
| `trn_customer_subscriptions_details` | `customer_subscription_id` + `end_date`    |
