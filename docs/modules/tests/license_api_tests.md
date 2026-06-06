# License API Module | Test Cases

**Project Code:** AXN-PM-01  
**Module Code:** API  
**File Path:** `/docs/modules/tests/license_api_tests.md`

---

## Test Case Summary Table

| TC ID              | Test Scenario                                         | Type        | Priority    | Expected Result                     | Status     |
|--------------------|-------------------------------------------------------|-------------|-------------|-------------------------------------|------------|
| AXN-PM-API-TC-001  | Valid db_name + existing device fingerprint (active)  | Functional  | P1-Critical | 200, response_code 202, access_rights returned | PENDING 🔄 |
| AXN-PM-API-TC-002  | Valid db_name + new device (with user details)        | Functional  | P1-Critical | 200, response_code 201, device registered | PENDING 🔄 |
| AXN-PM-API-TC-003  | New device without providing user details             | Validation  | P1-Critical | 422 — first_name, email required    | PENDING 🔄 |
| AXN-PM-API-TC-004  | Invalid db_name (not in system)                       | Negative    | P1-Critical | 404, response_code 404              | PENDING 🔄 |
| AXN-PM-API-TC-005  | Missing db_name field                                 | Validation  | P1-Critical | 422 validation error                | PENDING 🔄 |
| AXN-PM-API-TC-006  | Customer account is disabled (status=0)               | Functional  | P1-Critical | 200, response_code 4003             | PENDING 🔄 |
| AXN-PM-API-TC-007  | Device is disabled (product_users.status=0)           | Functional  | P1-Critical | 200, response_code 4005             | PENDING 🔄 |
| AXN-PM-API-TC-008  | Subscription is expired (end_date in past)            | Functional  | P1-Critical | 200, response_code 4001, expiry_date in data | PENDING 🔄 |
| AXN-PM-API-TC-009  | Subscription is in future (start_date is future)      | Functional  | P2-High     | 200, response_code 4002, start_date in data | PENDING 🔄 |
| AXN-PM-API-TC-010  | No subscription found for customer+product            | Functional  | P1-Critical | 200, response_code 4004             | PENDING 🔄 |
| AXN-PM-API-TC-011  | Active Trial period subscription                      | Functional  | P2-High     | 200, active_flow="Trial", access_rights present | PENDING 🔄 |
| AXN-PM-API-TC-012  | Active Free Service period                            | Functional  | P2-High     | 200, active_flow="Free Service"     | PENDING 🔄 |
| AXN-PM-API-TC-013  | Active AMC period                                     | Functional  | P2-High     | 200, active_flow="AMC"              | PENDING 🔄 |
| AXN-PM-API-TC-014  | Remaining days calculation accuracy                   | Functional  | P2-High     | remaining_days matches expected value | PENDING 🔄 |
| AXN-PM-API-TC-015  | Device IP fields updated on repeat call               | Functional  | P3-Medium   | product_users.public_ip updated in DB | PENDING 🔄 |

---

## Detailed Test Cases

---

### AXN-PM-API-TC-001

| Field             | Details                                                               |
|-------------------|-----------------------------------------------------------------------|
| **Test Case ID**  | AXN-PM-API-TC-001                                                     |
| **Module**        | License Verification API                                              |
| **Feature**       | Identify Customer Active Subscription, Known Device                 |
| **Test Type**     | Functional                                                            |
| **Priority**      | P1 Critical                                                         |
| **Pre-Conditions**| `customer_products` has a record with valid `db_name`. Active subscription exists. Device fingerprint already registered in `product_users`. |
| **Test Steps**    | 1. Send POST to `/api/identify-customer` with all required fields      |
| **Test Input Data**| `{"db_name": "test_client_db", "device_fingerprint": "EXISTING_FP", "public_ip": "1.2.3.4", "local_ip": "192.168.1.1", "device_uuid": "...", "cpu_processor_id": "...", "motherboard_serial_number": "...", "primary_disk_serial_number": "..."}` |
| **Expected Result**| `{"status": true, "response_code": 202, "data": { "access_rights": [...] }}` |
| **Actual Result** | _(Fill after execution)_                                              |
| **Status**        | PENDING 🔄                                                            |
| **Notes**         | Verify `updated_ip` and `updated_source='API'` in `product_users`    |

---

### AXN-PM-API-TC-008

| Field             | Details                                                               |
|-------------------|-----------------------------------------------------------------------|
| **Test Case ID**  | AXN-PM-API-TC-008                                                     |
| **Module**        | License Verification API                                              |
| **Feature**       | Expired Subscription Detection                                        |
| **Test Type**     | Functional                                                            |
| **Priority**      | P1 Critical                                                         |
| **Pre-Conditions**| Subscription exists. `trn_customer_subscriptions_details.end_date` is a past date. |
| **Test Steps**    | 1. Send valid POST request to `/api/identify-customer`               |
| **Expected Result**| `{"status": false, "response_code": 4001, "data": {"expiry_date": "YYYY-MM-DD"}}` |
| **Actual Result** | _(Fill after execution)_                                              |
| **Status**        | PENDING 🔄                                                            |
