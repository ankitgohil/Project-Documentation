# Module: License Verification API

---

## 1.0 Module Identity

| Field            | Details                                        |
|------------------|------------------------------------------------|
| **Module Name**  | License Verification API                       |
| **Module Code**  | API                                            |
| **Platform**     | Laravel (REST API Public Endpoint)           |
| **Status**       | Active                                         |
| **Developer**    | *(Assign)*                                     |
| **Created Date** | 2025-12-24                                     |

---

## 2.0 Purpose & Scope

This is the **external-facing API** consumed by client software (desktop/mobile apps) installed on customer premises. When the client application starts, it calls this endpoint with its hardware fingerprint and database name. The API verifies the device, validates the active subscription date window, and returns the exact hierarchical list of features the software should unlock. This module does **not** handle admin panel authentication.

---

## 3.0 User Roles & Permissions

| Consumer       | Action                  |
|----------------|-------------------------|
| Client App (Device) | POST `/api/identify-customer` |
| Admin Panel    | No direct access (internal routes only) |

---

## 4.0 Feature List

- [DONE] Validate `db_name` to identify the customer's product configuration
- [DONE] Check customer account is active (status = 1)
- [DONE] Register new device (if fingerprint not found) requires user details
- [DONE] Update existing device IP/hardware info on each call
- [DONE] Check device/user is not disabled (status check on product_users)
- [DONE] Find active subscription period (today between start_date and end_date)
- [DONE] Return Expired, Future, or Active status with custom response codes
- [DONE] Return full hierarchical access rights (Modules → Submodules → Permissions)
- [PLANNED] API Key authentication
- [PLANNED] Rate limiting (throttle per device fingerprint)
- [PLANNED] Response caching (Redis)

---

## 5.0 API Endpoint Documentation

---

### POST `/api/identify-customer`

| Field            | Value                                                    |
|------------------|----------------------------------------------------------|
| **Endpoint**     | `POST /api/identify-customer`                            |
| **Auth Required**| No (Public endpoint no API key currently)              |
| **Controller**   | `App\Http\Controllers\Api\SubscriptionApiController@identify` |
| **Description**  | Identifies device + validates subscription + returns access rights |

**Request Headers:**
```
Content-Type: application/json
Accept: application/json
```

**Request Body:**
| Parameter                  | Type    | Required      | Description                                             |
|---------------------------|---------|---------------|---------------------------------------------------------|
| `db_name`                  | String  | ✅ Always     | Database name in `customer_products.db_name`            |
| `device_fingerprint`       | String  | ✅ Always     | MD5/SHA of: UUID + CPU ID + Motherboard SN + Disk SN   |
| `public_ip`                | String  | ✅ Always     | Device's public IP address                              |
| `local_ip`                 | String  | ✅ Always     | Device's LAN IP address                                 |
| `device_uuid`              | String  | ✅ Always     | `wmic csproduct get uuid`                               |
| `cpu_processor_id`         | String  | ✅ Always     | `wmic cpu get ProcessorId`                              |
| `motherboard_serial_number`| String  | ✅ Always     | `wmic baseboard get serialnumber`                       |
| `primary_disk_serial_number`| String | ✅ Always     | `wmic diskdrive get SerialNumber`                       |
| `first_name`               | String  | ✅ New device | Required only for unregistered fingerprints             |
| `last_name`                | String  | ✅ New device | Required only for unregistered fingerprints             |
| `email`                    | String  | ✅ New device | Required only for unregistered fingerprints             |
| `phone`                    | String  | ✅ New device | Required only for unregistered fingerprints (10–13 digits) |

**Success Response (200 OK — Active Subscription):**
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

**Response Codes:**

| `response_code` | `status` | Scenario                                  |
|-----------------|----------|-------------------------------------------|
| `201`           | `true`   | New device registered, subscription active |
| `202`           | `true`   | Existing device updated, subscription active |
| `4001`          | `false`  | Subscription expired                       |
| `4002`          | `true`   | Subscription scheduled (future start date) |
| `4003`          | `false`  | Customer account is disabled               |
| `4004`          | `false`  | No active subscription found               |
| `4005`          | `false`  | This specific device is disabled           |
| `422`           | `false`  | Validation error (missing fields)          |
| `404`           | `false`  | `db_name` not found in system              |
| `500`           | `false`  | Internal server error                      |

---

## 6.0 Database Tables

| Table                               | Purpose                                       |
|-------------------------------------|-----------------------------------------------|
| `customer_products`                 | Lookup by `db_name` to identify customer+product |
| `customers`                         | Check customer account status                 |
| `product_users`                     | Device fingerprint registry                   |
| `trn_customer_subscriptions`        | Active subscription header                    |
| `trn_customer_subscriptions_details`| Phase timeline to find active date window     |
| `customer_modules`                  | Allowed modules for customer + product        |
| `customer_submodules`               | Allowed submodules (grouped by module)        |
| `customer_permission`               | Allowed permissions (grouped by submodule)    |

---

## 7.0 Business Logic Verification Flow

```
Step 1: Validate Required Fields
   → db_name and device_fingerprint required. 422 if missing.

Step 2: Lookup CustomerProduct by db_name
   → 404 if not found ("Invalid DB Name")

Step 3: Check Customer Status
   → customer.status == 0 → return 4003 (Account Disabled)

Step 4: Find or Register Device
   → Query product_users WHERE device_fingerprint + customer_id + product_id
   → If found + status == 0 → return 4005 (Device Disabled)
   → If found → UPDATE IP fields (log updated_ip, updated_source='API')
   → If NOT found → Validate first_name, last_name, email, phone → INSERT new product_user

Step 5: Find Active Subscription Header
   → TrnCustomerSubscription WHERE customer_id + product_id + status=1
   → If none → return 4004

Step 6: Find Active Date Window
   → Loop through details ordered by end_date DESC
   → Find detail WHERE today.between(start_date, end_date)
   → If no active window:
       → latest end_date in past → return 4001 (Expired)
       → latest start_date in future → return 4002 (Upcoming)

Step 7: Build Access Hierarchy (if active window found)
   → Fetch CustomerModules (status=1) WITH module name+code
   → Fetch CustomerSubmodules (status=1) WITH submodule name+code, group by module_id
   → Fetch CustomerPermissions (status=1) WITH permission name+code, group by submodule_id
   → Build nested map in memory:
       Module → [Submodule → [Permissions]]
   → Return 200 with access_rights array
```

---

## 8.0 Mobile Screens

N/A This is a pure JSON REST API. Client software handles UI.

---

## 9.0 Error Handling

| Scenario                        | Code | Response                                           |
|--------------------------------|------|----------------------------------------------------|
| Missing db_name or fingerprint  | 422  | JSON with `errors` object                          |
| db_name not in system           | 404  | `response_code: 404`                               |
| Customer disabled               | 200  | `response_code: 4003`                              |
| Device disabled                 | 200  | `response_code: 4005`                              |
| No subscription                 | 200  | `response_code: 4004`                              |
| Subscription expired            | 200  | `response_code: 4001` + `expiry_date`              |
| Upcoming subscription           | 200  | `response_code: 4002` + `start_date`               |
| New device missing user details | 422  | `errors` with field-level messages                 |
| Unhandled exception             | 500  | `response_code: 500`                               |

---

## 10.0 Security Notes ⚠️

> **Current Risk**: The endpoint has NO authentication mechanism beyond knowing a valid `db_name`. Anyone who can guess or discover a client's `db_name` can call this endpoint.

**Recommended**: Add an `api_secret` column to `customer_products` and require it in the request headers.

---

## 11.0 GitHub References

- **Branch Pattern**: `feature/api-[task]`, `fix/api-[issue]`
- **Controller**: `app/Http/Controllers/Api/SubscriptionApiController.php`
- **Route**: `routes/api.php` line 11

---

## 12.0 Change Log

| Date       | Developer | Change                                                    |
|------------|-----------|-----------------------------------------------------------|
| 2025-12-24 | —         | Initial identify-customer API endpoint created            |
| 2025-12-24 | —         | Added device fingerprint registration for new users       |
| 2025-12-24 | —         | Hierarchical access rights builder (Module→Sub→Permission)|
