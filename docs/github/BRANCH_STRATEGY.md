# Branch Strategy — AXN-042 Axone Client Portal

---

## Branch Naming Convention

All branches MUST follow this convention exactly. PRs with non-conforming branch names will be asked to rename.

| Branch Type | Pattern | Example |
|---|---|---|
| Main (Protected) | `main` | `main` — production-ready only |
| Development | `develop` | `develop` — integration branch |
| Feature | `feature/[module]-[task-name]` | `feature/auth-google-login` |
| Bug Fix | `fix/[module]-[issue-description]` | `fix/dashboard-null-pointer` |
| Hotfix | `hotfix/[brief-description]` | `hotfix/payment-timeout-crash` |
| Release | `release/v[major].[minor].[patch]` | `release/v2.4.0` |
| Chore / Config | `chore/[description]` | `chore/update-dependencies` |
| Documentation | `docs/[module]-update` | `docs/auth-module-update` |

---

## Protected Branches

| Branch | Who Can Push | Requires PR | Minimum Approvals |
|---|---|---|---|
| `main` | No one directly | Yes | 2 (Tech Lead + 1 Senior) |
| `develop` | No one directly | Yes | 1 |

---

## Merge Flow

```
feature/* ──▶ develop ──▶ release/* ──▶ main
fix/*     ──▶ develop
hotfix/*  ──▶ main (and back-merge to develop)
docs/*    ──▶ develop
chore/*   ──▶ develop
```

**Hotfixes only:** When a critical production bug requires immediate fix:
1. Branch from `main`: `git checkout -b hotfix/brief-description main`
2. Fix and test locally.
3. PR directly to `main` (requires 2 approvals).
4. After merge to `main`, immediately merge back to `develop`.

---

## Daily Workflow

```bash
# Sync with develop before starting any work
git checkout develop
git pull origin develop

# Create your branch
git checkout -b feature/auth-biometric-login

# Work and commit often
git add .
git commit -m "feat(auth): add biometric login screen in Flutter"

# Keep your branch up to date
git fetch origin
git rebase origin/develop

# Push and open PR
git push origin feature/auth-biometric-login
```

---

## Commit Message Convention (Conventional Commits)

Format: `type(scope): short description`

| Type | When to Use |
|---|---|
| `feat` | New feature |
| `fix` | Bug fix |
| `docs` | Documentation only |
| `chore` | Dependency updates, config, tooling |
| `refactor` | Code restructure, no behaviour change |
| `test` | Adding or updating tests |
| `hotfix` | Critical production fix |

Examples:
```
feat(auth): add Google OAuth login
fix(dashboard): resolve null pointer on revenue chart
docs(reports): update API endpoint documentation
chore: upgrade Laravel to v11.x
```
