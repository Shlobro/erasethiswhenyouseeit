# AGENTS.md

Rules for every AI agent working in this repository. Read this whole file before you start.
The company-wide rules in Skylyx Hub (`rules.md`, `how-we-work.md`) also apply. Where they
disagree with this file, this file wins for work in this repository.

## 1. Don't assume, ask

- If the request, the code or a person has not said it, you do not know it. Stop and ask.
- Do not fill a gap with a guess because it "sounds likely". Say what is missing.
- If a request is ambiguous, ask before writing code, not after.
- When you hand work back, say what you verified and what you did not.

## 2. Never work on `main`

- Never commit to, push to, rebase or reset `main`.
- Never force-push any branch.
- Never skip hooks (`--no-verify`) or bypass signing.

## 3. One feature, one worktree, one branch

Every feature or fix gets its own git worktree and its own branch. Do not reuse a worktree
for a second feature.

```bash
git fetch origin
git worktree add ../<repo-name>-worktrees/<branch-name> -b <branch-name> origin/main
cd ../<repo-name>-worktrees/<branch-name>
git submodule update --init --recursive
```

- **Branch names:** `feat/<short-name>`, `fix/<short-name>`, `chore/<short-name>`, in
  lowercase with hyphens. Example: `feat/offline-map-cache`.
- **Base:** always the latest `origin/main`, unless the user names another base.
- **Scope:** only the requested feature. Unrelated refactors, renames or "while I'm here"
  fixes are not allowed. Note them in the PR and ask instead.
- **Cleanup:** do not remove a worktree or delete a branch until the PR is merged and the user
  says so.
- **Other people's work:** another person or agent may be working in the same repository. Stage
  files by path (`git add path/to/file`), never `git add -A` or `git add .`. Never commit,
  revert or delete a file you did not change.

## 4. No code file over 2000 lines, ever

- No hand-written source file may be over **2000 lines**, blank lines and comments included.
- **Before every commit**, check the line count of each file you changed. If a change would take
  a file over 2000 lines, split it into smaller modules with clear responsibilities first.
- **Files already over the limit:** do not make them longer. If your change has to touch one,
  tell the user and propose a split. Do not do a large split without a yes.
- **Excluded:** generated code, vendored or third-party SDK code, lock files, data files
  (JSON/CSV/etc.) and binaries. If you are not sure whether a file counts, ask.

## 5. Every feature ends with a Pull Request, never a merge

When the feature is done, push the branch and open a PR. The user's request for a feature is
permission to push that feature branch and open its PR. It is **not** permission to merge.

- **Never merge a PR**: not your own, not anyone else's, not even if checks pass.
- Never enable auto-merge.
- Before opening the PR:
  - The project builds.
  - Existing tests pass.
  - A bug fix comes with a test that fails without the fix.
  - Every changed file passes the 2000-line rule.
  - Visual changes have screenshots (section 6).
- Use this PR description:

```markdown
## What changed and why
<short summary, and the request it answers>

## How to run / test it
<exact commands, and what to click or look at>

## Screenshots
<required for any UI/UX change, see section 6; otherwise "No visual change">

## Not done / not verified
<anything skipped, untested, or assumed; "Nothing" only if truly nothing>

## Notes for AGENTS.md
<copy of any entry you added to Agent-Feedback.md; otherwise "None">
```

After opening the PR, give the user the PR link and stop. Wait for their review.

## 6. Any visual change needs screenshots in the PR

A change to anything the user can see (layout, colours, text, icons, screens, flows, animations)
must have screenshots in the PR as proof.

- Take **before and after** screenshots of every screen or state that changed.
- Commit them to `docs/screenshots/<branch-name>/` with descriptive names, such as
  `before-map-screen.png` and `after-map-screen.png`. Embed them in the PR description with
  relative links.
- If you cannot capture a screenshot (no emulator, no device, the app will not run), **do not
  open the PR as finished**. Say so in the PR under "Not done / not verified" and tell the user.
  Never fake, mock up or reuse an old screenshot as proof.

## 7. Report important findings in `Agent-Feedback.md`

If you notice something every future agent should know, add it to `Agent-Feedback.md`.
Examples: a project rule that is missing here, a pitfall, a build quirk, a convention the code
follows, or a mistake you or another agent made. The user reviews that file and decides what to
move into this one.

- **Never edit `AGENTS.md` yourself.** Only the user changes it.
- Add new entries **at the end** of `Agent-Feedback.md` only. Never edit or remove existing
  entries.
- Commit the entry on your feature branch as a separate commit
  (`docs: agent feedback - <topic>`), and copy it into the PR's "Notes for AGENTS.md" section.
- Use the entry format given at the top of `Agent-Feedback.md`.

## 8. Commits and code hygiene

- Make small, focused commits with clear messages, for example
  `feat: add offline map cache`, `fix: crash on lost video link`.
- Never commit secrets, keys, tokens or `.env` files. Secrets go in a git-ignored `.env`.
- The team works on macOS and Windows:
  - Refer to files by paths relative to the repository, never by absolute path.
  - Scripts must run under bash (macOS) and Git Bash (Windows).
  - Do not assume `python3` exists.
- Before adding a dependency, model or copied code, check its licence and say what it is in
  the PR.

## 9. Safety

- Never send commands to a real aircraft, controller or connected hardware without the user's
  explicit yes for that specific action.
- Any flight test follows the Skylyx Hub `flight-safety.md`. An agent never plans or starts one
  on its own.
