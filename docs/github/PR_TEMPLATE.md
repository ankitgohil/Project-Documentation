# PR Template | Product Management System

The file below is saved as `.github/pull_request_template.md` in the repository root so **every PR automatically loads this structure** on GitHub.

---

## PR Assignments

| Field       | Who Sets It                       | Purpose                                                     |
|-------------|-----------------------------------|-------------------------------------------------------------|
| **Assignee**  | Developer who wrote the code    | Ownership they handle all requested changes               |
| **Reviewer**  | Team Lead / Senior Dev          | Quality gate must approve before merge                    |
| **Label**     | Developer (or auto via Actions) | Categorize PR for filtering and reporting                   |
| **Milestone** | Team Lead                       | Link PR to a sprint or release milestone                    |

---

## Label Quick Reference

See [BRANCH_STRATEGY.md](./BRANCH_STRATEGY.md#4-pr-labels-reference) for the full label list with hex colors.

---

## Template (`.github/pull_request_template.md`)

```markdown
## Summary
What does this PR do? (1-2 sentences)

## TMS Task
Task #: ____   Repository: ____

## Type of Change
- [ ] New Feature
- [ ] Bug Fix
- [ ] Hotfix
- [ ] Refactor
- [ ] Documentation

## Testing Done
- [ ] Tested locally
- [ ] Unit tests pass
- [ ] No console errors

## Screenshots (if UI change)
(Paste before/after screenshots here)

## Checklist
- [ ] Branch is up-to-date with develop/main
- [ ] Code reviewed by self
- [ ] No debug code / console.log left
- [ ] Dependent changes merged upstream
```
