# Local Setup Guide — AXN-042 Axone Client Portal

Follow every step in order. Do not skip steps.

---

## Prerequisites

| Tool | Version | Install |
|---|---|---|
| PHP | 8.2+ | [php.net](https://php.net) or Homebrew |
| Composer | 2.x | [getcomposer.org](https://getcomposer.org) |
| MySQL | 8.0+ | [mysql.com](https://dev.mysql.com) or DBngin |
| Node.js | 18+ | [nodejs.org](https://nodejs.org) |
| Redis | 7.x | Homebrew or Docker |
| Flutter | 3.19+ | [flutter.dev](https://flutter.dev) |
| Git | 2.x | Pre-installed on macOS |

---

## Step 1 — Clone the Repository

```bash
git clone git@github.com:axoneinfotech/axn-client-portal.git
cd axn-client-portal
```

---

## Step 2 — Install PHP Dependencies

```bash
composer install
```

---

## Step 3 — Configure Environment

```bash
cp .env.example .env
php artisan key:generate
```

Open `.env` and fill in values from the Bitwarden vault (ask Tech Lead for access):

```env
DB_DATABASE=axn_portal
DB_USERNAME=your_db_user
DB_PASSWORD=your_db_password

REDIS_HOST=127.0.0.1
REDIS_PORT=6379

MAIL_MAILER=mailgun
MAILGUN_DOMAIN=axoneinfotech.com
MAILGUN_SECRET=        # from Bitwarden

AWS_ACCESS_KEY_ID=     # from Bitwarden
AWS_SECRET_ACCESS_KEY= # from Bitwarden
AWS_DEFAULT_REGION=ap-south-1
AWS_BUCKET=axn-portal-uploads-staging

FIREBASE_SERVER_KEY=   # from Bitwarden
GOOGLE_CLIENT_ID=      # from Bitwarden
GOOGLE_CLIENT_SECRET=  # from Bitwarden
```

---

## Step 4 — Set Up Database

```bash
mysql -u root -p -e "CREATE DATABASE axn_portal CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;"
php artisan migrate
php artisan db:seed
```

The seeder creates:
- 1 Super Admin user: `admin@axoneinfotech.com` / `Admin@1234`
- Sample roles and permissions
- 10 sample users

---

## Step 5 — Start Services

Open 3 terminal tabs:

**Tab 1 — Laravel Backend:**
```bash
php artisan serve
# Runs at http://localhost:8000
```

**Tab 2 — Queue Worker:**
```bash
php artisan queue:work --queue=reports,default
```

**Tab 3 — Redis (if not running as service):**
```bash
redis-server
```

---

## Step 6 — Mobile Setup (Flutter)

```bash
cd mobile/
flutter pub get
flutter run
```

> Make sure `.env` in the mobile folder has `API_BASE_URL=http://localhost:8000/api/v1`

---

## Verify Installation

Visit [http://localhost:8000/api/v1/health](http://localhost:8000/api/v1/health) — should return `{"status": "ok"}`.

Try logging in:
```bash
curl -X POST http://localhost:8000/api/v1/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"admin@axoneinfotech.com","password":"Admin@1234"}'
```

---

## Common Issues

| Problem | Fix |
|---|---|
| `php artisan migrate` fails | Check `DB_*` values in `.env`. Ensure MySQL is running. |
| `SQLSTATE[HY000] [2002] No such file` | MySQL socket issue — use `DB_HOST=127.0.0.1` not `localhost` |
| Redis connection refused | Start Redis: `redis-server` or `brew services start redis` |
| Composer memory error | `COMPOSER_MEMORY_LIMIT=-1 composer install` |
| Flutter `pub get` fails | Run `flutter doctor` and fix any issues first |
