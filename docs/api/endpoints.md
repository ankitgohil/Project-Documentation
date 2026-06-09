# API Endpoints | Product Managemnt System

All API endpoints for the Product Management System.

***

## Authentication



| Title     | Name                   | Data                        |
| --------- | ---------------------- | --------------------------- |
| Ankit 001 | Gohil Ankit Rajeshbhai | ipdated data file on li9ve  |
|           |                        |                             |



All **web routes** use Laravel **session-based authentication** (Laravel Breeze / Sanctum). The `auth` middleware protects all admin routes.

The **external API** (`/api/identify-customer`) is a **public endpoint** — no Bearer token or session required.

***

## External API Endpoints

### POST `/api/identify-customer`

**Purpose:** License verification and device registration for client software.

**Auth Required:** No

**Request Body:**

```json
{
  "db_name": "customer_db_name",
  "device_fingerprint": "hash_of_hardware_ids",
  "public_ip": "1.2.3.4",
  "local_ip": "192.168.1.10",
  "device_uuid": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "cpu_processor_id": "BFEBFBFF000906EA",
  "motherboard_serial_number": "MB-SN-001",
  "primary_disk_serial_number": "DISK-SN-001",
  "first_name": "John",
  "last_name": "Doe",
  "email": "john@example.com",
  "phone": "9876543210"
}
```

> `first_name`, `last_name`, `email`, `phone` are required only for new (unregistered) devices.

**Response Codes:**

| `response_code` | `status` | Meaning                       |
| --------------- | -------- | ----------------------------- |
| `201`           | `true`   | New device registered, active |
| `202`           | `true`   | Device updated, active        |
| `4001`          | `false`  | Subscription expired          |
| `4002`          | `true`   | Subscription is future        |
| `4003`          | `false`  | Customer account disabled     |
| `4004`          | `false`  | No active subscription        |
| `4005`          | `false`  | Device disabled               |
| `422`           | `false`  | Validation error              |
| `404`           | `false`  | db\_name not found            |
| `500`           | `false`  | Server error                  |

**Success Response Example:**

```json
{
  "status": true,
  "response_code": 202,
  "message": "Device details updated. Your subscription is active.",
  "data": {
    "active_flow": "Normal",
    "start_date": "2025-01-01",
    "end_date": "2026-01-01",
    "remaining_days": 210,
    "subscription_id": 5,
    "access_rights": [
      {
        "id": 7,
        "name": "Counter Wise",
        "code": "MOD_CW",
        "submodules": [
          {
            "id": 13,
            "name": "Orderwise",
            "code": "SUB_OW",
            "permissions": [
              {
                "id": 14,
                "name": "Item Sales Report",
                "code": "RP_R_ISR"
              }
            ]
          }
        ]
      }
    ]
  }
}
```

***

## Internal AJAX Endpoints (Web, Auth Required)

These endpoints are called by the React frontend and require an active admin session.

| Method | Endpoint                                   | Returns                                  |
| ------ | ------------------------------------------ | ---------------------------------------- |
| GET    | `/api/products/{product}/plans`            | Array of active subscription plans       |
| GET    | `/api/plans/{plan}/details`                | Plan with modules/submodules/permissions |
| GET    | `/api/products/{product}/structure`        | Full product module hierarchy            |
| GET    | `/api/products/{product}/modules`          | Modules for a specific product           |
| GET    | `/api/modules/{module}/submodules`         | Submodules for a specific module         |
| GET    | `/api/products/{product}/hierarchy`        | Full Module→Sub→Permission tree          |
| GET    | `/locations/countries`                     | All countries                            |
| GET    | `/locations/states/{country}`              | States for a country                     |
| GET    | `/locations/cities/{state}`                | Cities for a state                       |
| GET    | `/reports/product-customer-report/details` | Subscription detail data (JSON)          |

***

## Web Resource Routes Summary

| Resource            | Route Prefix           | CRUD Methods Supported                      |
| ------------------- | ---------------------- | ------------------------------------------- |
| Roles               | `/roles`               | index, create, store, edit, update, destroy |
| Users               | `/users`               | index, create, store, edit, update, destroy |
| Countries           | `/countries`           | index, create, store, edit, update, destroy |
| States              | `/states`              | index, create, store, edit, update, destroy |
| Cities              | `/cities`              | index, create, store, edit, update, destroy |
| Products            | `/products`            | index, create, store, edit, update, destroy |
| Subscription Plans  | `/subscription-plans`  | index, create, store, edit, update, destroy |
| Modules             | `/modules`             | index, create, store, edit, update, destroy |
| Submodules          | `/submodules`          | index, create, store, edit, update, destroy |
| Product Permissions | `/product-permissions` | index, create, store, edit, update, destroy |
| Customers           | `/customers`           | index, create, store, edit, update, destroy |
| Subscriptions       | `/subscriptions`       | index, create, store, edit, update, destroy |