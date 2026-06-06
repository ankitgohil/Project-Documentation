# Module: Location Masters (Countries, States, Cities)

---

## 1.0 Module Identity

| Field            | Details                    |
|------------------|----------------------------|
| **Module Name**  | Location Masters           |
| **Module Code**  | LOC                        |
| **Platform**     | Laravel + React (Inertia)  |
| **Status**       | Active                     |
| **Created Date** | 2025-12-16                 |

---

## 2.0 Purpose & Scope

Provides the geographic lookup data (Countries, States, Cities) used in Customer and User forms. The CRUD for these master records is in the admin panel, and the dynamic AJAX lookups are used in forms to build dependent dropdowns. This module has no business transaction logic.

---

## 4.0 Feature List

- [DONE] Countries CRUD
- [DONE] States CRUD (linked to country)
- [DONE] Cities CRUD (linked to state)
- [DONE] Dynamic AJAX endpoint: states by country
- [DONE] Dynamic AJAX endpoint: cities by state

---

## 5.0 API Endpoints

| Method | Route                               | Auth | Controller                         | Purpose                  |
|--------|-------------------------------------|------|------------------------------------|--------------------------|
| GET    | `/countries`                        | ✅   | `CountryController@index`          | List countries           |
| GET    | `/states`                           | ✅   | `StateController@index`            | List states              |
| GET    | `/cities`                           | ✅   | `CityController@index`             | List cities              |
| GET    | `/locations/countries`              | ✅   | `LocationController@getCountries`  | AJAX: all countries      |
| GET    | `/locations/states/{country}`       | ✅   | `LocationController@getStates`     | AJAX: states by country  |
| GET    | `/locations/cities/{state}`         | ✅   | `LocationController@getCities`     | AJAX: cities by state    |

---

## 6.0 Database Tables

| Table       | Key Columns                         |
|-------------|-------------------------------------|
| `countries` | `id`, `name`, `code`                |
| `states`    | `id`, `country_id`, `name`, `code`  |
| `cities`    | `id`, `state_id`, `name`            |

---

## 11.0 Change Log

| Date       | Developer | Change                                    |
|------------|-----------|-------------------------------------------|
| 2025-12-16 | —         | Countries, States, Cities tables created  |
| 2025-12-16 | —         | AJAX location endpoints added             |
