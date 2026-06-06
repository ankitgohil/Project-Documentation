# Module: Products, Modules, Submodules & Permissions

---

## 1.0 Module Identity

| Field            | Details                                           |
|------------------|---------------------------------------------------|
| **Module Name**  | Product Catalog & Access Structure                |
| **Module Code**  | PROD                                              |
| **Platform**     | Laravel + React (Inertia)                         |
| **Status**       | Active                                            |
| **Developer**    | *(Assign)*                                        |
| **Created Date** | 2025-12-16                                        |

---

## 2.0 Purpose & Scope

Defines the catalog of software products sold by Axone Infotech and their granular feature hierarchy (Modules → Submodules → Permissions). This hierarchy acts as the master template from which Subscription Plans and Customer Access Rights are derived. This module does **not** handle billing or customer assignment.

---

## 3.0 User Roles & Permissions

| Action              | Admin | Manager |
|---------------------|-------|---------|
| View Products        | ✅    | ✅      |
| Create/Edit Products | ✅    | ❌      |
| Manage Modules       | ✅    | ❌      |
| Manage Submodules    | ✅    | ❌      |
| Manage Permissions   | ✅    | ❌      |
| Bulk Status Toggle   | ✅    | ❌      |

---

## 4.0 Feature List

- [DONE] Product CRUD (name, code, department, database type, language, description)
- [DONE] Product hierarchy API (`GET /api/products/{product}/hierarchy`)
- [DONE] Module CRUD with product linkage
- [DONE] Module toggle status + bulk status update
- [DONE] Submodule CRUD with module linkage
- [DONE] Submodule toggle status + bulk status update
- [DONE] Permission (product_permissions) CRUD with submodule linkage
- [DONE] Permission toggle status + bulk status update
- [PLANNED] Module import/export

---

## 5.0 API Endpoints

| Method | Route                                      | Auth | Controller                            | Purpose                         |
|--------|--------------------------------------------|------|---------------------------------------|---------------------------------|
| GET    | `/products`                                | ✅   | `ProductController@index`             | List products                   |
| POST   | `/products`                                | ✅   | `ProductController@store`             | Create product                  |
| PUT    | `/products/{id}`                           | ✅   | `ProductController@update`            | Update product                  |
| DELETE | `/products/{id}`                           | ✅   | `ProductController@destroy`           | Delete product                  |
| GET    | `/api/products/{product}/hierarchy`        | ✅   | `ProductController@getHierarchy`      | Full Module→Sub→Perm tree (JSON)|
| GET    | `/modules`                                 | ✅   | `ModuleController@index`              | List modules                    |
| POST   | `/modules/bulk-status`                     | ✅   | `ModuleController@bulkStatusUpdate`   | Bulk toggle module status       |
| POST   | `/modules/{module}/toggle-status`          | ✅   | `ModuleController@toggleStatus`       | Toggle single module            |
| GET    | `/submodules`                              | ✅   | `SubmoduleController@index`           | List submodules                 |
| GET    | `/api/products/{product}/modules`          | ✅   | `SubmoduleController@getModulesByProduct` | AJAX: modules for a product |
| GET    | `/api/modules/{module}/submodules`         | ✅   | `ProductPermissionController@getSubmodulesByModule` | AJAX: submodules for a module |
| GET    | `/product-permissions`                     | ✅   | `ProductPermissionController@index`   | List permissions                |
| POST   | `/product-permissions/bulk-status`         | ✅   | `ProductPermissionController@bulkStatusUpdate` | Bulk toggle permissions |

---

## 6.0 Database Tables

### `products`
| Column          | Type     | Notes                                          |
|-----------------|----------|------------------------------------------------|
| `name`          | VARCHAR  | e.g. "Billing Pro"                             |
| `code`          | VARCHAR  | Short unique code, e.g. "BP"                   |
| `department`    | TINYINT  | 1=Laravel, 2=Desktop, 3=MVC, 4=Mobile App      |
| `database_type` | VARCHAR  | e.g. MySQL, MSSQL                              |
| `language`      | VARCHAR  | e.g. PHP, C#                                   |
| `status`        | TINYINT  | 1=Active, 0=Inactive                           |

### `product_modules`
| Column       | Type | Notes                                               |
|--------------|------|-----------------------------------------------------|
| `product_id` | FK   | → `products.id`                                     |
| `module_id`  | FK   | → `modules.id` (the master module list)             |
| `status`     | TINYINT | 1=Active (filters API responses)               |

### `submodules`
| Column      | Type    | Notes                              |
|-------------|---------|------------------------------------|
| `module_id` | FK      | → `modules.id` (parent module)     |
| `name`      | VARCHAR |                                    |
| `code`      | VARCHAR | Used by API in access_rights output|
| `status`    | TINYINT |                                    |

### `product_permissions`
| Column        | Type    | Notes                          |
|---------------|---------|--------------------------------|
| `module_id`   | FK      | → `modules.id`                 |
| `submodule_id`| FK      | → `submodules.id`              |
| `name`        | VARCHAR | e.g. "Item Sales Report"       |
| `code`        | VARCHAR | e.g. "RP_R_ISR"                |
| `status`      | TINYINT |                                |

---

## 7.0 Business Logic

1. Product hierarchy is: `Product → ProductModule → Submodule → ProductPermission`.
2. A Module can be shared across products via `product_modules` pivot.
3. Only Active (`status=1`) modules/submodules/permissions are served to subscription plan selectors and to the API.
4. The module `code` and permission `code` are the machine-readable identifiers used by client software to enable/disable features.

---

## 9.0 Error Handling

| Scenario          | Response                   |
|-------------------|----------------------------|
| Duplicate code    | 422 Validation unique rule |
| Invalid parent FK | 422 `exists` rule          |

---

## 10.0 GitHub References

- **Branch Pattern**: `feature/prod-[task]`
- **Controllers**: `ProductController.php`, `ModuleController.php`, `SubmoduleController.php`, `ProductPermissionController.php`

---

## 11.0 Change Log

| Date       | Developer | Change                                         |
|------------|-----------|------------------------------------------------|
| 2025-12-16 | —         | Products table and CRUD created                |
| 2025-12-17 | —         | Submodules and product_permissions CRUD created|
| 2025-12-17 | —         | Bulk status update + individual toggle added   |
