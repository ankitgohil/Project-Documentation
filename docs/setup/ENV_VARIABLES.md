# Environment Variables Reference — AXN-042

> ⚠️ This file lists variable NAMES and descriptions only. NEVER commit actual values here.
> All actual values are stored in the team Bitwarden vault under "AXN-042 Environment".

---

## Application

| Variable | Required | Description |
|---|---|---|
| `APP_NAME` | Yes | Application name (e.g. "Axone Portal") |
| `APP_ENV` | Yes | `local` / `staging` / `production` |
| `APP_KEY` | Yes | Laravel app encryption key. Generate: `php artisan key:generate` |
| `APP_DEBUG` | Yes | `true` for local/staging, `false` for production |
| `APP_URL` | Yes | Full base URL of the app |

---

## Database

| Variable | Required | Description |
|---|---|---|
| `DB_CONNECTION` | Yes | Always `mysql` |
| `DB_HOST` | Yes | MySQL host (use `127.0.0.1` locally) |
| `DB_PORT` | Yes | MySQL port (default `3306`) |
| `DB_DATABASE` | Yes | Database name |
| `DB_USERNAME` | Yes | MySQL username |
| `DB_PASSWORD` | Yes | MySQL password — from Bitwarden |

---

## Redis / Cache / Queue

| Variable | Required | Description |
|---|---|---|
| `REDIS_HOST` | Yes | Redis host |
| `REDIS_PASSWORD` | No | Redis password (null for local) |
| `REDIS_PORT` | Yes | Default `6379` |
| `CACHE_DRIVER` | Yes | `redis` for staging/prod, `array` for local testing |
| `QUEUE_CONNECTION` | Yes | `redis` for staging/prod, `sync` for local testing |
| `SESSION_DRIVER` | Yes | `redis` |

---

## Email (Mailgun)

| Variable | Required | Description |
|---|---|---|
| `MAIL_MAILER` | Yes | `mailgun` |
| `MAILGUN_DOMAIN` | Yes | Mailgun sending domain — from Bitwarden |
| `MAILGUN_SECRET` | Yes | Mailgun API key — from Bitwarden |
| `MAIL_FROM_ADDRESS` | Yes | Default sender email |
| `MAIL_FROM_NAME` | Yes | Default sender name |

---

## AWS S3 (File Storage)

| Variable | Required | Description |
|---|---|---|
| `AWS_ACCESS_KEY_ID` | Yes | AWS IAM access key — from Bitwarden |
| `AWS_SECRET_ACCESS_KEY` | Yes | AWS IAM secret — from Bitwarden |
| `AWS_DEFAULT_REGION` | Yes | e.g. `ap-south-1` |
| `AWS_BUCKET` | Yes | S3 bucket name (different per environment) |

---

## Firebase (Push Notifications)

| Variable | Required | Description |
|---|---|---|
| `FIREBASE_SERVER_KEY` | Yes | Firebase Cloud Messaging server key — from Bitwarden |

---

## Google OAuth

| Variable | Required | Description |
|---|---|---|
| `GOOGLE_CLIENT_ID` | Yes | Google OAuth Client ID — from Bitwarden |
| `GOOGLE_CLIENT_SECRET` | Yes | Google OAuth Client Secret — from Bitwarden |
| `GOOGLE_REDIRECT_URI` | Yes | OAuth callback URL (must match Google Console) |

---

## Razorpay (Payments — Planned)

| Variable | Required | Description |
|---|---|---|
| `RAZORPAY_KEY` | Planned | Razorpay API Key — from Bitwarden |
| `RAZORPAY_SECRET` | Planned | Razorpay API Secret — from Bitwarden |
