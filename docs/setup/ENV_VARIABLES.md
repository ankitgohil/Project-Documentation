# Environment Variables Reference

All `.env` keys used by this project. **Never store actual secret values in this file or in the repository.**

Actual values are managed via the `.env` file (not committed to Git) or via the server's environment configuration.

---

## Application Settings

| Variable        | Type    | Required | Description                              | Example                      |
|-----------------|---------|----------|------------------------------------------|------------------------------|
| `APP_NAME`      | String  | ✅       | Application display name                 | `Product Management`         |
| `APP_ENV`       | String  | ✅       | Environment: `local`, `staging`, `production` | `local`               |
| `APP_KEY`       | String  | ✅       | Laravel encryption key (auto-generated)  | `base64:...`                 |
| `APP_DEBUG`     | Boolean | ✅       | Show debug errors. **Must be `false` in production** | `true`   |
| `APP_URL`       | URL     | ✅       | Base URL of the application              | `http://localhost`           |
| `APP_TIMEZONE`  | String  | ❌       | Server timezone                          | `Asia/Kolkata`               |
| `APP_LOCALE`    | String  | ❌       | Default locale                           | `en`                         |

---

## Database Settings

| Variable      | Type    | Required | Description               | Example          |
|---------------|---------|----------|---------------------------|------------------|
| `DB_CONNECTION`| String | ✅       | DB driver: `mysql`        | `mysql`          |
| `DB_HOST`     | String  | ✅       | Database server host       | `127.0.0.1`      |
| `DB_PORT`     | Integer | ✅       | Database port              | `3306`           |
| `DB_DATABASE` | String  | ✅       | Database name              | `product_management` |
| `DB_USERNAME` | String  | ✅       | Database user              | `root`           |
| `DB_PASSWORD` | String  | ✅       | Database password          | *(secret)*       |

---

## Cache & Session Settings

| Variable         | Type   | Required | Description                                | Example      |
|------------------|--------|----------|--------------------------------------------|--------------|
| `CACHE_STORE`    | String | ❌       | Cache driver: `file`, `redis`, `database`  | `database`   |
| `SESSION_DRIVER` | String | ❌       | Session driver: `file`, `database`, `redis`| `database`   |
| `SESSION_LIFETIME`| Integer| ❌      | Session lifetime in minutes                | `120`        |

---

## Mail Settings

| Variable       | Type    | Required | Description                    | Example             |
|----------------|---------|----------|--------------------------------|---------------------|
| `MAIL_MAILER`  | String  | ✅       | Mailer driver: `smtp`, `log`   | `smtp`              |
| `MAIL_HOST`    | String  | ✅       | SMTP host                      | `smtp.mailtrap.io`  |
| `MAIL_PORT`    | Integer | ✅       | SMTP port                      | `2525`              |
| `MAIL_USERNAME`| String  | ✅       | SMTP username                  | *(secret)*          |
| `MAIL_PASSWORD`| String  | ✅       | SMTP password                  | *(secret)*          |
| `MAIL_ENCRYPTION`| String| ❌      | TLS/SSL                        | `tls`               |
| `MAIL_FROM_ADDRESS`| Email| ✅     | Sender email address           | `noreply@axone.com` |
| `MAIL_FROM_NAME`| String | ✅      | Sender name                    | `Axone Infotech`    |

---

## Queue Settings

| Variable          | Type   | Required | Description                               | Example   |
|-------------------|--------|----------|-------------------------------------------|-----------|
| `QUEUE_CONNECTION`| String | ❌       | Queue driver: `sync`, `database`, `redis` | `database`|

---

## Redis Settings (If Used)

| Variable          | Type    | Required | Description         | Example     |
|-------------------|---------|----------|---------------------|-------------|
| `REDIS_HOST`      | String  | ❌       | Redis server host   | `127.0.0.1` |
| `REDIS_PASSWORD`  | String  | ❌       | Redis password      | `null`      |
| `REDIS_PORT`      | Integer | ❌       | Redis port          | `6379`      |

---

## Notes

- Copy `.env.example` to `.env` to start: `cp .env.example .env`
- Run `php artisan key:generate` to fill `APP_KEY` automatically.
- **Never commit `.env` to the repository.** It is listed in `.gitignore`.
- For production, set `APP_DEBUG=false` and `APP_ENV=production`.
