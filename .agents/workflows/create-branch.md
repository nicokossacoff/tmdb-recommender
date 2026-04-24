---
description: Create a new branch synchronized with develop
---

1. Ask the user if it's a new feature or a hot-fix branch. Ask for the name of the branch.
2. Switch to the default branch to start fresh.
// turbo
3. Run `git checkout develop`
4. Pull the latests changes.
// turbo
5. Run `git pull`
6. Create the branch. If it's a new feature branch, the branch name should start with `feature/`. If it's a hot-fix branch, the branch name should start with `hot-fix/`.
// turbo
7. Run `git checkout -b [complete-branch-name]`
8. Push to remote.
// turbo
9. Run `git push -u origin [complete-branch-name]`