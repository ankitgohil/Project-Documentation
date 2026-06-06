# Subscription Management | Test Cases

**Project Code:** AXN-PM-01  
**Module Code:** SUB  
**File Path:** `/docs/modules/tests/subscriptions_tests.md`

---

## Test Case Summary Table

| TC ID              | Test Scenario                                                  | Type       | Priority    | Expected Result                              | Status     |
|--------------------|----------------------------------------------------------------|------------|-------------|----------------------------------------------|------------|
| AXN-PM-SUB-TC-001  | Create subscription with Trial + Free + Normal plan            | Functional | P1-Critical | 3 detail rows created with correct date chain | PENDING 🔄 |
| AXN-PM-SUB-TC-002  | Create subscription when active one already exists             | Negative   | P1-Critical | Error: "already has active subscription"      | PENDING 🔄 |
| AXN-PM-SUB-TC-003  | Create subscription with AMC plan (no trial)                   | Functional | P2-High     | 1 AMC detail row created                      | PENDING 🔄 |
| AXN-PM-SUB-TC-004  | Deactivate subscription sets status=0                          | Functional | P1-Critical | subscription.status=0, customer_product.status=0 | PENDING 🔄 |
| AXN-PM-SUB-TC-005  | Access rights sync on create modules saved correctly         | Functional | P1-Critical | customer_modules rows match selected_modules  | PENDING 🔄 |
| AXN-PM-SUB-TC-006  | Access rights sync on update old modules deleted, new inserted | Functional | P1-Critical | Old records gone, new records present        | PENDING 🔄 |
| AXN-PM-SUB-TC-007  | One-time plan calculates end_date as +10 years                 | Functional | P2-High     | end_date = purchase_date + 10 years           | PENDING 🔄 |
| AXN-PM-SUB-TC-008  | Monthly plan calculates end_date as +1 month                   | Functional | P2-High     | end_date = start_date + 1 month               | PENDING 🔄 |
| AXN-PM-SUB-TC-009  | Yearly plan calculates end_date as +1 year                     | Functional | P2-High     | end_date = start_date + 1 year                | PENDING 🔄 |
| AXN-PM-SUB-TC-010  | DB credentials saved to customer_products on create            | Functional | P2-High     | db_name, db_host, etc. stored correctly       | PENDING 🔄 |
| AXN-PM-SUB-TC-011  | Create without selecting any modules (empty array)             | Functional | P3-Medium   | Subscription created, customer_modules empty  | PENDING 🔄 |
| AXN-PM-SUB-TC-012  | Required fields missing (no customer_id)                       | Validation | P1-Critical | 422 validation error                          | PENDING 🔄 |

---

## Detailed Test Cases

---

### AXN-PM-SUB-TC-001

| Field             | Details                                                                              |
|-------------------|--------------------------------------------------------------------------------------|
| **Test Case ID**  | AXN-PM-SUB-TC-001                                                                    |
| **Module**        | Subscription Management                                                              |
| **Feature**       | Create Subscription Multi-Phase Timeline                                           |
| **Test Type**     | Functional                                                                           |
| **Priority**      | P1 Critical                                                                        |
| **Pre-Conditions**| Customer exists (active). Product exists (active). Subscription plan exists with `is_trial=true, trial_day=30, is_free_service=true, free_service_day=15, is_amc=false, term=2 (Yearly)`. |
| **Test Steps**    | 1. Go to `/subscriptions/create` 2. Select customer, product, plan 3. Set purchase_date = today 4. Submit |
| **Expected Result**| 3 rows in `trn_customer_subscriptions_details`: <br> Row 1: TYPE_TRIAL, start=today, end=today+30 <br> Row 2: TYPE_FREE_SERVICE, start=today+30, end=today+45 <br> Row 3: TYPE_NORMAL, start=today+45, end=today+45+1year |
| **Actual Result** | _(Fill after execution)_                                                             |
| **Status**        | PENDING 🔄                                                                           |
| **Notes**         | Verify exact dates in DB after submission.                                            |
