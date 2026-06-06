# Branch Strategy | Product Management System

---

## 1. Branch Naming Convention

Branch names are tied to **TMS task numbers** for full traceability.

### Format

```
{ProjectCode}{TaskNumber}{ProjectCode}-{type}-{short-description}
```

### Types

| Type       | When to Use                   |
|------------|-------------------------------|
| *(none)*   | New feature / task            |
| `bugfix`   | Bug fix                       |
| `hotfix`   | Urgent production fix         |
| `refactor` | Code restructure              |

### Examples

```
V187V-implement-google-authenticator
V125V-bugfix-user-token-not-refresh
V200V-hotfix-payment-gateway-crash
V215V-refactor-order-module
```

> ⚠️ **Rule:** One branch per TMS task. **Never commit directly to `main` or `develop`.** Always push via a branch and raise a PR.

---

## 2. Core GitHub Workflow

Follow these steps **in order** for every task/feature. Every step maps to a TMS action.

| #  | Action          | Description                                                      | Command / Tool                        |
|----|-----------------|------------------------------------------------------------------|---------------------------------------|
| 01 | Create Repo     | Create on GitHub with README + .gitignore                        | `github.com/new`                      |
| 02 | Clone           | Clone repo to your machine                                       | `git clone <url>`                     |
| 03 | Sync Main       | Always pull latest before branching                              | `git pull origin main`                |
| 04 | Create Branch   | Create TMS-based branch name                                     | `git checkout -b V187V-feat`          |
| 05 | Stage Changes   | Stage specific or all changed files                              | `git add .` OR `git add <file>`       |
| 06 | Commit          | Write a meaningful Conventional Commit message                   | `git commit -m "feat: msg"`           |
| 07 | Push Branch     | Push your branch to remote                                       | `git push origin V187V-feat`          |
| 08 | Create PR       | Open PR on GitHub, fill template, assign reviewer                | GitHub UI → Compare & PR              |
| 09 | PR Review       | Reviewer approves, requests changes, or comments                 | GitHub PR Review Panel                |
| 10 | Merge PR        | Squash & Merge into develop/main, delete branch                  | GitHub UI → Merge PR                  |
| 11 | Close TMS Task  | Add PR number + repo name to TMS task before closing             | TMS → Close Task                      |

---

## 3. Commit Message Standard (Conventional Commits)

Consistent commit messages make the project history readable, filterable, and auto-releasable.

### Format

```
<type>(<scope>): <short summary>
```

### Types

| Type       | When to Use                                          |
|------------|------------------------------------------------------|
| `feat`     | New feature                                          |
| `fix`      | Bug fix                                              |
| `hotfix`   | Urgent production fix                                |
| `docs`     | Documentation only                                   |
| `style`    | Formatting, missing semicolons, etc.                 |
| `refactor` | Code change, no new feature or fix                   |
| `perf`     | Performance improvement                              |
| `test`     | Adding or updating tests                             |
| `chore`    | Build process, dependency update, config             |
| `ci`       | CI/CD related changes                                |

### Examples

```
feat(auth): add Google Authenticator 2FA support
fix(token): resolve user token not refreshing on expiry
hotfix(payment): fix payment gateway crash on timeout
docs(readme): update setup instructions for new devs
chore(deps): upgrade Laravel from 10 to 11
```

### ✅ Good Commit
```
feat(order): add bulk order status update API endpoint
```

### ❌ Bad Commits Avoid These
```
"fixed stuff" / "changes" / "wip" / "done"
```
> These provide **zero context** in history.

---

## 4. PR Labels Reference

Apply labels to **every PR** for traceability and dashboard filtering.

| Label              | Hex Color | When to Use                                            |
|--------------------|-----------|--------------------------------------------------------|
| 🟥 `bugfix`         | `#d73a4a` | Fixes a confirmed bug in the codebase                  |
| 🟦 `feature`        | `#0e8a16` | Adds a new feature or user-facing functionality        |
| 🟧 `enhancement`    | `#ff8c00` | Improves or extends an existing feature                |
| 🟨 `documentation`  | `#ffd33d` | Updates or adds project documentation                  |
| 🟪 `refactor`       | `#6f42c1` | Code restructure with no behavior change               |
| 🟩 `performance`    | `#2cbe4e` | Improves speed, memory, or resource usage              |
| 🟫 `wip`            | `#b60205` | Work in Progress not ready for review                  |
| 🟦 `ready for review`| `#1d76db` | Code complete, ready for peer review                  |
| 🟥 `hotfix`         | `#b60205` | Urgent production fix needs immediate merge            |
| 🟪 `UI/UX`          | `#f4c2c2` | Changes to user interface or experience                |
| 🟧 `test`           | `#fbca04` | Adds or updates test cases                             |
| 🔴 `breaking change`| `#d93f0b` | Breaks backward compatibility communicate first        |
| 🟫 `security`       | `#b60205` | Addresses a security vulnerability                     |
| 🟩 `chore`          | `#d4c5f9` | Config, dependency, or cleanup tasks                   |
| 🟩 `ready for merge`| `#2cbe4e` | PR reviewed and approved safe to merge                 |
| 🟨 `dependencies`   | `#e4e669` | Updates or changes package dependencies                |
