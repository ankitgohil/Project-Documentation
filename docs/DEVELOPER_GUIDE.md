# DEVELOPER GUIDE — AXN-042 Axone Client Portal

> Welcome to the team. Read this document fully before writing a single line of code.

---

## 1. Onboarding Checklist

| Step | Action | Done? |
|---|---|---|
| 1 | Read PROJECT_OVERVIEW.md | ☐ |
| 2 | Clone the repository from GitHub | ☐ |
| 3 | Follow LOCAL_SETUP.md to configure your environment | ☐ |
| 4 | Copy `.env.example` → `.env` and fill in values from Bitwarden vault | ☐ |
| 5 | Run `php artisan migrate --seed` | ☐ |
| 6 | Read BRANCH_STRATEGY.md | ☐ |
| 7 | Read all module docs in /docs/modules/ | ☐ |
| 8 | Be added to GitHub repo by Tech Lead | ☐ |
| 9 | Be added to project's GitHub Project Board | ☐ |
| 10 | Complete your first task (assigned by PM/Tech Lead) | ☐ |

---

## 2. Repository Structure

```
axn-client-portal/
├── app/
│   ├── Http/Controllers/
│   ├── Models/
│   ├── Services/
│   └── Jobs/
├── database/migrations/
├── routes/api.php
├── docs/                  ← All documentation lives here
│   ├── PROJECT_OVERVIEW.md
│   ├── CHANGELOG.md
│   ├── DEVELOPER_GUIDE.md
│   ├── modules/
│   ├── api/
│   ├── database/
│   ├── setup/
│   └── github/
├── .env.example
└── README.md
```

---

## 3. Daily Development Workflow

```bash
# Every morning — sync with develop
git checkout develop
git pull origin develop

# Start a task
git checkout -b feature/[module]-[task-name]

# Commit often with clear messages
git commit -m "feat(auth): add refresh token rotation"

# Push and open PR
git push origin feature/[module]-[task-name]
# → Open PR on GitHub using PR_TEMPLATE.md
```

---

## 4. Code Standards

- **PSR-12** coding style enforced via PHP-CS-Fixer.
- **Service Layer**: Business logic lives in `app/Services/`, not in controllers.
- **No direct DB queries in controllers**: Use Eloquent models and repositories.
- **Validation**: Always use Form Request classes (`php artisan make:request`).
- **No hardcoded strings**: Use config files or `.env` variables.
- **All API routes** must be versioned under `/api/v1/`.

---

## 5. Running the Project Locally

See [LOCAL_SETUP.md](setup/LOCAL_SETUP.md) for the full step-by-step guide.

Quick commands:
```bash
php artisan serve          # Start backend
php artisan queue:work     # Start queue worker
php artisan horizon        # Start Horizon (queue dashboard)
```

---

## 6. Who to Contact

| Question | Contact |
|---|---|
| Architecture / Tech decisions | Rahul Mehta (Tech Lead) |
| Task clarification | Neeha Kulkarni (PM) |
| Code review | Priya Sharma (Senior Dev) |
| Deployment / DevOps | Rahul Mehta |
| QA / Bug reports | Karan Desai |

---

## 7. Key Policies

- **Never commit to `main` directly.** All changes go through PRs.
- **Never self-merge a PR.** Minimum 1 approval required on `develop`, 2 on `main`.
- **Docs are mandatory.** PRs without updated documentation will be rejected.
- **Secrets stay out of the repo.** Use `.env` only — never commit actual keys.
- **Test before you PR.** Run all P1 and P2 test cases on your feature before opening a PR.
