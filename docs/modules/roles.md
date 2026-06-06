# Module: Roles

---

## 1.0 Module Identity

| Field            | Details                    |
|------------------|----------------------------|
| **Module Name**  | Role Management            |
| **Module Code**  | ROLE                       |
| **Platform**     | Laravel + React (Inertia)  |
| **Status**       | Active                     |
| **Created Date** | 2025-12-13                 |

---

## 2.0 Purpose & Scope

Manages admin panel user roles (e.g., Admin, Manager, Staff). Roles are assigned to users and control panel-level access. Currently roles are stored as named records fine-grained permission enforcement per route is planned.

---

## 4.0 Feature List

- [DONE] List roles
- [DONE] Create role
- [DONE] Edit role
- [DONE] Delete role (with soft delete)
- [PLANNED] Role-based middleware enforcement on web routes

---

## 5.0 API Endpoints

| Method | Route             | Auth | Purpose        |
|--------|-------------------|------|----------------|
| GET    | `/roles`          | ✅   | List roles     |
| POST   | `/roles`          | ✅   | Create role    |
| PUT    | `/roles/{id}`     | ✅   | Update role    |
| DELETE | `/roles/{id}`     | ✅   | Delete role    |

---

## 6.0 Database Tables

### `roles`
| Column      | Notes              |
|-------------|--------------------|
| `name`      | Role name          |
| `deleted_at`| Soft delete        |

### `permissions`
| Column    | Notes                       |
|-----------|-----------------------------|
| `name`    | Permission identifier       |
| `role_id` | FK → `roles.id` (planned pivot) |

---

## 11.0 Change Log

| Date       | Developer | Change                           |
|------------|-----------|----------------------------------|
| 2025-12-13 | —         | Roles table + CRUD created       |
| 2025-12-13 | —         | Soft delete added                |
