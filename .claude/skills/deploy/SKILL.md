---
name: deploy
description: Commit and push the working tree, ensuring any new HTML tool pages have a corresponding entry in pages.json so they show up in the searchable index.
---

# deploy

Use this skill when the user runs `/deploy`. It commits and pushes outstanding changes in this repo, and makes sure every HTML tool page is registered in `pages.json`.

## Steps

1. **Check status.** Run `git status` and `git diff` (staged + unstaged) to see what's changing. Note any untracked or modified `*.html` files at the repo root, excluding `index.html` (that's the index page itself, not a tool).

2. **Reconcile `pages.json`.** Read `pages.json`. For every tool HTML file present in the working tree (added or already tracked), confirm there's a matching entry with `file`, `title`, and `description`. If any are missing:
   - Read the HTML file to derive a sensible `title` (prefer the `<title>` tag or main `<h1>`) and a one-sentence `description` of what the tool does.
   - Add an entry to `pages.json`. Keep the file as a JSON array, 2-space indent, matching the existing style.
   - Stage the updated `pages.json` along with the new HTML.

3. **Commit.** Follow the repo's commit conventions (see recent `git log`): short conventional-style subject like `feat: add <tool>` for new tools, `fix:` / `chore:` / `docs:` as appropriate. Stage files explicitly by name — do not use `git add -A` or `git add .` (avoids picking up `.DS_Store` and similar). Use a HEREDOC for the commit message and include the standard `Co-Authored-By` trailer.

4. **Push.** Run `git push` to the current branch's upstream. If the branch has no upstream, push with `-u origin <branch>`. Report the resulting commit SHA and confirm the push succeeded.

## Notes

- `index.html` and `CNAME` are infrastructure, not tools — never add them to `pages.json`.
- If there are no changes to commit, say so and stop; do not create an empty commit.
- If the pre-commit hook (or any hook) fails, fix the underlying issue and create a new commit. Never use `--no-verify`.
