# Module: Customer Management

---

## 1.0 Module Identity

| Field            | Details                            |
|------------------|------------------------------------|
| **Module Name**  | Customer Management                |
| **Module Code**  | CUST                               |
| **Platform**     | Laravel + React (Inertia)          |
| **Status**       | Active                             |
| **Developer**    | *(Assign)*                         |
| **Created Date** | 2025-12-17                         |

---

## 2.0 Purpose & Scope

Manages the client organizations (customers) to whom Axone Infotech sells products and subscriptions. Each customer record stores company identity, contact information, geographic location, and account status. The customer record is the starting point for assigning subscriptions and tracking licenses. This module does **not** handle subscription logic (see `subscriptions.md`).

---

## 3.0 User Roles & Permissions

| Action         | Admin | Developer | QA |
|----------------|-------|-----------|-----|
| View List       | ✅    | ✅        | ✅  |
| Create          | ✅    | ✅        | ❌  |
| Edit            | ✅    | ✅        | ❌  |
| Toggle Status   | ✅    | ✅        | ❌  |
| Delete          | ✅    | ❌        | ❌  |

---

## 4.0 Feature List

- [DONE] List all customers with search (company name, first/last name, email, phone) + sort + pagination
- [DONE] Create customer with full location hierarchy (Country → State → City)
- [DONE] Edit customer details
- [DONE] Toggle Active / Inactive status (soft toggle, not delete)
- [DONE] Delete customer record
- [DONE] Audit trail (created_by, created_ip, updated_by, updated_ip, source)
- [PLANNED] Customer Portal (customer-facing login)

---

## 5.0 API Endpoints (Web Routes)

| Method | Route                                    | Auth | Controller                       | Purpose                    |
|--------|------------------------------------------|------|----------------------------------|----------------------------|
| GET    | `/customers`                             | ✅   | `CustomerController@index`       | List customers (paginated) |
| GET    | `/customers/create`                      | ✅   | `CustomerController@create`      | Show create form           |
| POST   | `/customers`                             | ✅   | `CustomerController@store`       | Save new customer          |
| GET    | `/customers/{id}/edit`                   | ✅   | `CustomerController@edit`        | Show edit form             |
| PUT    | `/customers/{id}`                        | ✅   | `CustomerController@update`      | Save changes               |
| DELETE | `/customers/{id}`                        | ✅   | `CustomerController@destroy`     | Delete customer            |
| POST   | `/customers/{customer}/toggle-status`    | ✅   | `CustomerController@toggleStatus`| Toggle active/inactive     |

---

## 6.0 Database Tables

### `customers`
| Column         | Type         | Required | Notes                                 |
|----------------|--------------|----------|---------------------------------------|
| `id`           | BIGINT PK    | ✅       | Auto-increment                        |
| `company_name` | VARCHAR(100) | ✅       | Client company name                   |
| `first_name`   | VARCHAR(100) | ✅       | Contact person first name             |
| `last_name`    | VARCHAR(100) | ✅       | Contact person last name              |
| `email`        | VARCHAR(100) | ✅       | Unique per customer                   |
| `phone`        | VARCHAR(13)  | ✅       | 10–13 digits                          |
| `address_line1`| VARCHAR(150) | ✅       | Primary address                       |
| `address_line2`| VARCHAR(150) | ❌       | Optional                              |
| `country_id`   | FK           | ✅       | → `countries.id`                      |
| `state_id`     | FK           | ✅       | → `states.id`                         |
| `city_id`      | FK           | ✅       | → `cities.id`                         |
| `postal_code`  | INTEGER      | ✅       | PIN/ZIP code                          |
| `status`       | TINYINT      | ✅       | 1 = Active, 0 = Inactive              |
| `remark`       | VARCHAR(255) | ❌       | Optional internal notes               |
| `created_by`   | FK → users   | Auto     | Set from `auth()->id()`              |
| `created_ip`   | VARCHAR      | Auto     | Set from `$request->ip()`            |
| `created_source`| VARCHAR     | Auto     | Always `'web'` for panel entries     |
| `updated_by`   | FK → users   | Auto     |                                       |
| `updated_ip`   | VARCHAR      | Auto     |                                       |
| `deleted_at`   | TIMESTAMP    | ❌       | Soft deletes support                  |

**Relationships:**
- `customers` → has many → `customer_products`
- `customers` → belongs to → `countries`, `states`, `cities`

---

## 7.0 Business Logic

1. On create — email is validated for uniqueness across all customers.
2. State dropdown is filtered based on selected country (async: `GET /locations/states/{country}`).
3. City dropdown is filtered based on selected state (async: `GET /locations/cities/{state}`).
4. `toggle-status` sets `status = 1` (active) or `status = 0` (inactive).
5. When a customer is set to **inactive**, the License API (`identify-customer`) returns `response_code: 4003` blocking all their device logins.
6. Deleting a customer uses Laravel's soft delete (if configured on the model) or hard delete.

---

## 8.0 Mobile Screens

N/A Admin panel only.

---

## 9.0 Error Handling

| Scenario                    | Outcome                                    |
|-----------------------------|---------------------------------------------|
| Duplicate email             | 422 "The email has already been taken"    |
| Phone not 10–13 digits      | 422 Validation error                      |
| Missing required fields     | 422 Field-level errors                    |
| Customer not found          | 404 Laravel `findOrFail` throws ModelNotFoundException |

---

## 10.0 GitHub References

- **Branch Pattern**: `feature/cust-[task]`, `fix/cust-[issue]`
- **Related Routes**: `routes/web.php` lines 41–42

---

## 11.0 Change Log

| Date       | Developer | Change                                             |
|------------|-----------|----------------------------------------------------|
| 2025-12-17 | —         | Initial Customer CRUD created                       |
| 2025-12-16 | —         | Location columns (country/state/city) added to users |
