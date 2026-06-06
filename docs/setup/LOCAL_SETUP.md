# Local Development Setup Guide

Step-by-step instructions to set up this project on your local machine.

---

## Prerequisites

Ensure the following are installed before starting:

| Tool         | Required Version | Install Guide                            |
|--------------|-----------------|------------------------------------------|
| PHP          | 8.2+            | [php.net](https://php.net/downloads.php) |
| Composer     | 2.x             | [getcomposer.org](https://getcomposer.org) |
| Node.js      | 20.x            | [nodejs.org](https://nodejs.org)         |
| MySQL/MariaDB| 8.0+ / 10.5+    | Or use WAMP/XAMPP/Laragon on Windows     |
| Git          | Any             | [git-scm.com](https://git-scm.com)       |

---

## Setup Steps

### 1. Clone the Repository
```bash
git clone https://github.com/Axone-Infotech/Product-management.git
cd Product-management
```

### 2. Install PHP Dependencies
```bash
composer install
```

### 3. Install Node Dependencies
```bash
npm install
```

### 4. Configure Environment
```bash
cp .env.example .env
```

Open `.env` and update:
```ini
APP_NAME="Product Management"
APP_ENV=local
APP_URL=http://localhost/Product-management/public

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=product_management
DB_USERNAME=root
DB_PASSWORD=
```

> See [ENV_VARIABLES.md](./ENV_VARIABLES.md) for all available environment variables.

### 5. Generate Application Key
```bash
php artisan key:generate
```

### 6. Create Database
In MySQL:
```sql
CREATE DATABASE product_management CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

### 7. Run Migrations
```bash
php artisan migrate
```

### 8. (Optional) Seed Test Data
```bash
php artisan db:seed
```
> Note: Check if seeders exist before running.

### 9. Build Frontend Assets
For development (with hot reload):
```bash
npm run dev
```

For production build:
```bash
npm run build
```

### 10. Start the Development Server
```bash
php artisan serve
```
Then open: [http://127.0.0.1:8000](http://127.0.0.1:8000)

---

## Using WAMP64 (Windows)

If using WAMP64 (as per the current dev environment at `c:\wamp64\www\Product-management`):

1. Place the project in `C:\wamp64\www\Product-management\`
2. Start WAMP64 (Apache + MySQL)
3. Access via: `http://localhost/Product-management/public/`
4. Run `npm run dev` in a separate terminal for asset compilation
5. No need for `php artisan serve` Apache handles PHP

---

## All-in-One Dev Command

The `composer.json` includes a `dev` script that starts all services together:
```bash
composer run dev
```
This runs concurrently:
- `php artisan serve`
- `php artisan queue:listen`
- `php artisan pail --timeout=0` (real-time logs)
- `npm run dev`

---

## Quick Reference

```bash
# Fresh setup shortcut (copies .env, generates key, migrates, builds)
composer run setup

# Start dev environment
composer run dev

# Run tests
composer run test

# Clear all caches
php artisan optimize:clear
```
