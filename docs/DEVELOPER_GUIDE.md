# Developer Guide | Product Management System

Welcome to the Product Management (License Server) project. Read this guide completely before writing any code.

---

## 1. What is This System?

This is a **centralized License & Subscription Management Server** built for Axone Infotech. It:
- Manages a catalog of client-facing software **Products**.
- Defines granular feature access as **Modules → Submodules → Permissions**.
- Creates **Subscription Plans** with pricing, billing terms, and trial periods.
- Links **Customers** to products via **Subscriptions**.
- Exposes a **REST API** (`/api/identify-customer`) that client software can call to validate their license and retrieve their allowed features.

---

## 2. System Request Lifecycle

### Web Admin Panel (React + Inertia)
```
Browser Request
    ↓
Laravel Router (routes/web.php)  ← protected by 'auth' middleware (Sanctum session)
    ↓
Controller (app/Http/Controllers/)
    ↓
Eloquent Model + MySQL Database
    ↓
Inertia::render('Page/Component', $data)  ← sends JSON props to React
    ↓
React Component (resources/js/Pages/)  ← renders HTML in browser (SPA)
```

### License Verification API (Client Software)
```
Client Application (remote device)
    ↓
POST /api/identify-customer
    ↓
Laravel Router (routes/api.php)  ← NO auth middleware (public endpoint)
    ↓
SubscriptionApiController::identify()
    ↓
1. Validate db_name + device_fingerprint
2. Lookup CustomerProduct by db_name
3. Check Customer status (active/disabled)
4. Register or update ProductUser (device record)
5. Find active TrnCustomerSubscriptionDetail (where today is between start_date and end_date)
6. If active → build Module → Submodule → Permission hierarchy from customer_* tables
7. Return JSON response with access_rights
```

---

## 3. Important Files

| File | Purpose |
|------|---------|
| `routes/web.php` | All admin panel routes |
| `routes/api.php` | External API route |
| `routes/auth.php` | Auth routes (login, register, password reset) |
| `app/Http/Controllers/Api/SubscriptionApiController.php` | **Core** the license verification API logic |
| `app/Http/Controllers/SubscriptionController.php` | **Core** subscription creation with phase timeline |
| `app/Models/TrnCustomerSubscription.php` | Subscription header model |
| `app/Models/TrnCustomerSubscriptionDetail.php` | Subscription phase (Trial/Free/AMC/Normal) model |
| `app/Models/CustomerProduct.php` | Links customer to product + DB credentials |
| `app/Models/ProductUser.php` | Device fingerprint registry |
| `resources/js/Layouts/AuthenticatedLayout.jsx` | Main sidebar layout wrapping all authenticated pages |
| `resources/js/Components/ClientDataTable.jsx` | Reusable paginated sortable data table |
| `tailwind.config.js` | Brand colors: primary `#24695c`, secondary `#ba895d` |

---

## 4. Critical Business Logic

### Subscription Phase Timeline
When a subscription is created, the system **chains dates sequentially**:
```
Purchase Date
     ↓
[TRIAL]       start = Purchase Date    end = start + trial_days
     ↓
[FREE SERVICE] start = Trial End       end = start + free_service_days
     ↓
[AMC / NORMAL] start = Free End        end = calculated by term (Monthly/Yearly/One-Time)
```

### Access Rights Snapshot
On every subscription create/update, `syncAccessConfig()` in `SubscriptionController`:
1. **Deletes** all existing records from `customer_modules`, `customer_submodules`, `customer_permission` for that customer+product.
2. **Re-inserts** only the newly selected modules/submodules/permissions.

> ⚠️ This means editing a subscription re-writes all access rights. Never modify these tables directly.

---

## 5. Important Dependencies

| Package | Used For |
|---------|----------|
| `inertiajs/inertia-laravel` | Bridge between Laravel controllers and React pages |
| `tightenco/ziggy` | Makes Laravel named routes available in JavaScript |
| `laravel/sanctum` | Session-based auth for web + token-based for API |
| `@inertiajs/react` | React bindings for Inertia (useForm, usePage, Link) |
| `@headlessui/react` | Accessible UI components (Dropdown, Modal, etc.) |
| `@heroicons/react` | SVG icon set |
| `use-debounce` | Debounces search inputs to reduce API calls |

---

## 6. High-Risk Areas Be Very Careful

| Area | Risk | What Could Break |
|------|------|-----------------|
| `SubscriptionController::store()` | Date calculation for Trial/Free/AMC chain | Wrong billing periods for all future customers |
| `SubscriptionApiController::identify()` | Core API response structure | Client software will break if JSON shape changes |
| `syncAccessConfig()` | Deletes and re-creates all customer access rights | Temporary access loss if transaction fails |
| `CustomerProduct` `db_name` field | Used as the API lookup key | Changing db_name will lock out the client's device |
| Migrations | Schema is tightly coupled | Adding non-nullable columns without defaults will break existing data |

---

## 7. Local Development Setup

See [setup/LOCAL_SETUP.md](./setup/LOCAL_SETUP.md) for step-by-step instructions.

---

## 8. Branch & PR Workflow

See [github/BRANCH_STRATEGY.md](./github/BRANCH_STRATEGY.md) for branch naming rules.

Always use the PR template in [github/PR_TEMPLATE.md](./github/PR_TEMPLATE.md).

---

## 9. Documentation Policy

> **A PR cannot be merged if the relevant `/docs/modules/[module].md` file is not updated.**

Follow the Axone Infotech Documentation Standard defined in `ProjectDocSystem.docx`.
