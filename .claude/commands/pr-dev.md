---
description: Push akshay-dev and create a pull request into dev.
---

1. Verify the current branch is `akshay-dev`.
   - Stop if it isn't.

2. Check for uncommitted changes.
   - If present:
     - Generate a concise Conventional Commit message.
     - Show it for approval.
     - Stage all files.
     - Commit.

3. Push `akshay-dev` to origin.

4. Create a Pull Request:

Base branch:
dev

Head branch:
akshay-dev

5. Generate:

PR Title:
- A concise Conventional Commit title.

PR Description:
- Write it like a well-written Git commit message.
- Explain:
  - what changed,
  - why it changed,
  - any important implementation details,
  - notable behavioral changes.
- Keep it concise.
- Do NOT use:
  - Summary sections
  - Testing sections
  - Checklists
  - Bullet lists
  - Markdown headings
- End with:

Co-Authored-By: Claude <model name> <noreply@anthropic.com>

6. Return the PR URL after creation.