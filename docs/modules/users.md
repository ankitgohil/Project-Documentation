# Module: User Management

---

## 1.0 Module Identity

| Field            | Details                            |
|------------------|------------------------------------|
| **Module Name**  | User Management                    |
| **Module Code**  | USR                                |
| **Platform**     | Laravel + React (Inertia)          |
| **Status**       | Active                             |
| **Developer**    | *(Assign)*                         |
| **Created Date** | 2025-12-13                         |

---

## 2.0 Purpose & Scope

Manages the internal admin panel user accounts. Each user belongs to a role, and has a status (active/inactive). This module is for managing **staff accounts** that log into the admin panel it is not related to customer end-users or device registrations.

---

## 3.0 Feature List

- [DONE] List users with pagination, search, sort
- [DONE] Create user (name, email, phone, role, status, location)
- [DONE] Edit user
- [DONE] Toggle user active/inactive status
- [DONE] Soft delete user

---

## 5.0 API Endpoints

| Method | Route                | Auth | Controller                    | Purpose           |
|--------|----------------------|------|-------------------------------|-------------------|
| GET    | `/users`             | ✅   | `UserController@index`        | List users        |
| POST   | `/users`             | ✅   | `UserController@store`        | Create user       |
| PUT    | `/users/{id}`        | ✅   | `UserController@update`       | Update user       |
| DELETE | `/users/{id}`        | ✅   | `UserController@destroy`      | Delete user       |

---

## 6.0 Database Tables

### `users`
| Column              | Notes                                  |
|---------------------|----------------------------------------|
| `name`              | Full name                              |
| `email`             | Unique, used for login                 |
| `password`          | Bcrypt hashed                          |
| `role_id`           | FK → `roles.id`                        |
| `phone`             | Contact number                         |
| `status`            | 1=Active, 0=Inactive                   |
| `country_id`, `state_id`, `city_id` | Location                |
| `deleted_at`        | Soft delete                            |

---

## 11.0 Change Log

| Date       | Developer | Change                        |
|------------|-----------|-------------------------------|
| 2025-12-13 | —         | Users table + CRUD created    |
| 2025-12-13 | —         | Soft delete + status added    |
