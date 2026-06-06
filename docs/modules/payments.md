# Module: Payment Gateway (PAYM)

---

## 1. Module Identity

| Field | Value |
|---|---|
| **Module Name** | Payment Gateway |
| **Module ID** | PAYM |
| **Platform** | Laravel API + Flutter |
| **Status** | Planned |
| **Assigned Developer** | TBD |
| **Creation Date** | 2024-06-20 |
| **Last Updated** | 2024-06-20 |

---

## 2. Purpose & Scope

The Payment Gateway module will handle invoice creation, payment processing via Razorpay, payment status tracking, and receipt generation. It will serve Admin and Manager roles for invoicing clients and tracking revenue. Does NOT cover payroll or internal expense management.

---

## 3. User Roles & Permissions

| Role | Create Invoice | View All Invoices | View Own Invoices | Process Payment | Issue Refund |
|---|---|---|---|---|---|
| Super Admin | ✅ | ✅ | ✅ | ✅ | ✅ |
| Admin | ✅ | ✅ | ✅ | ✅ | ✅ |
| Manager | ✅ | ❌ | ✅ | ✅ | ❌ |
| User | ❌ | ❌ | ✅ | ✅ (own) | ❌ |

---

## 4. Feature List

- [PLANNED] Create Invoice
- [PLANNED] Razorpay payment link generation
- [PLANNED] Payment status webhook handling
- [PLANNED] Payment receipt generation (PDF)
- [PLANNED] Refund processing
- [PLANNED] Revenue reconciliation report

---

## 5. API Endpoints

> TODO: Endpoints to be defined during sprint planning for v2.5.0.

---

## 6. Database Tables

> TODO: Schema to be designed. Expected tables: `invoices`, `payments`, `refunds`.

---

## 7. Business Logic

> TODO: To be documented when development begins.

---

## 8. Mobile Screens

> TODO: Screens to be designed in Figma before development.

---

## 9. Error Handling

> TODO

---

## 10. GitHub References

- Branch pattern: `feature/paym-[task]`, `fix/paym-[issue]`
- Milestone: v2.5.0

---

## 11. Change Log

| Date | Developer | Change |
|---|---|---|
| 2024-06-20 | Neeha Kulkarni | Module stub created for planning |
