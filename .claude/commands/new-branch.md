---
description: Create and switch to a new Git branch.
---

1. Ask me for the new branch name if I haven't already provided one.

2. Validate that the branch name follows our naming convention (for example:
   - feature/<name>
   - fix/<name>
   - hotfix/<name>
   - chore/<name>
   - refactor/<name>
   - or any branch name I explicitly provide).

3. Verify that my working tree is clean.
   - If there are uncommitted changes, stop and explain why creating a new branch now may not be appropriate.
   - Do not automatically stash or commit changes.

4. Fetch the latest changes from origin.

5. Checkout the latest `main` branch.

6. Pull the latest changes from `origin/main`.

7. Create the new branch from `main`.

8. Switch to the new branch.

9. Verify the current branch.

10. Print a confirmation including:
    - Previous branch
    - New branch
    - Base branch (`main`)

Do not push the new branch unless I explicitly ask.