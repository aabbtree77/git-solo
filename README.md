> "I have no job, no wife, no kids and no plan." 
>
> [\- CycleCruza (2026)](https://www.youtube.com/watch?v=uOF1ieP8VUs&t=2s)


_... Sum up essential git commands from this whole process. Do not focus on analytics and side detours, just what was essential, to branch, to push, PR, and proper merge/fetch in the end. Always trace the state of: working directory, staging area, local repo, remote repo, and what commands moves what in between where. Output this in Markdown code so I can put it on github as a memo, not browser rendered snippets of Markdown. Strive for clarity, not thoroughness, I want this nailed as a solid repeatable workflow from A to Z engraved in stone._

# Git Feature Branch Workflow (A → Z)

Think of Git as five places where your code can live:

```
Working Directory  →  Staging Area  →  Local Repository  →  Remote Repository (GitHub)
      (files)            (index)           (commits)              (commits)

                                         ^
                                         |
                                   Pull Request
                                         |
                                         v
                                   Remote main
```

Every command simply moves changes from one place to another.

---

# 0. Start from an up-to-date main

```bash
git checkout main
git pull
```

Moves:

```
GitHub main ─────► Local main
```

---

# 1. Create a feature branch

```bash
git checkout -b my-feature
```

Moves:

```
Local main ─────► Local my-feature
```

Nothing changes on GitHub yet.

---

# 2. Edit files

Just edit normally.

State:

```
Working Directory
    modified files
```

Check:

```bash
git status
```

---

# 3. Stage changes

```bash
git add .
```

Moves:

```
Working Directory ─────► Staging Area
```

---

# 4. Commit

```bash
git commit -m "Describe the change"
```

Moves:

```
Staging Area ─────► Local Repository
```

You can repeat:

```
edit
add
commit
edit
add
commit
...
```

as many times as needed.

---

# 5. Push the branch

First push:

```bash
git push -u origin my-feature
```

Later pushes:

```bash
git push
```

Moves:

```
Local Repository ─────► GitHub branch
```

---

# 6. Open Pull Request

On GitHub:

```
Compare & Pull Request
```

Verify:

```
base: main
compare: my-feature
```

Press:

```
Create Pull Request
```

Review.

Press:

```
Merge Pull Request
Confirm Merge
```

Moves:

```
GitHub feature branch ─────► GitHub main
```

This creates a merge commit on GitHub.

---

# 7. Update local main

```bash
git fetch origin
git checkout main
git reset --hard origin/main
```

Moves:

```
GitHub main ─────► Local main
```

Your local main now exactly matches GitHub.

---

# 8. Delete feature branch

Local:

```bash
git branch -d my-feature
```

Remote:

```bash
git push origin --delete my-feature
```

Result:

```
main
```

is the only remaining branch.

---

# 9. Verify

```bash
git status
```

Expected:

```
On branch main
nothing to commit, working tree clean
```

Check branch pointers:

```bash
git log -1 --oneline --decorate
```

Expected:

```
(HEAD -> main, origin/main)
```

meaning local and remote point to the exact same commit.

---

# The Four Essential Commands

Edit:

```bash
git add .
git commit -m "Message"
git push
```

Sync:

```bash
git pull
```

---

# The One Mental Model

```
edit
    │
    ▼
Working Directory

git add
    │
    ▼
Staging Area

git commit
    │
    ▼
Local Repository

git push
    │
    ▼
GitHub Branch

Merge Pull Request
    │
    ▼
GitHub main

git fetch
git reset --hard origin/main
    │
    ▼
Local main
```

If you always know **where your latest changes currently live**, Git becomes straightforward.




_Did we use switch at some point and if yes what was that, anything extra? Is this only for solo development?_

Yes, there are two points worth adding.

## `git switch`

`git switch` is a newer, more focused alternative to `git checkout` for changing branches.

Examples:

```bash
git switch main
```

instead of

```bash
git checkout main
```

Create and switch:

```bash
git switch -c my-feature
```

instead of

```bash
git checkout -b my-feature
```

Many tutorials now recommend `switch` because it only deals with branches, whereas `checkout` historically did several unrelated things (switching branches, restoring files, checking out commits).

Either style is fine. Just be consistent.

And yes, the workflow I gave is essentially the canonical solo-developer workflow.

For solo development, it's hard to beat:

```
main
  │
  ├── feature A
  ├── feature B
  └── feature C
```

Every feature:

Branch from main
Commit as much as you like
Push
Open PR
Merge
Delete branch
Update local main

Repeat.

For teams, the picture is slightly different because other people are also changing main.

That introduces things like:

pulling frequently
rebasing or merging main into your feature branch
resolving merge conflicts
code reviews
CI checks before merge

But the skeleton is exactly the same:

```
main
   │
   └── feature branch
           │
      commit...
           │
         push
           │
     Pull Request
           │
         review
           │
         merge
           │
      update main
```

So I'd keep your memo exactly as it is. It's an excellent "engraved in stone" workflow for a solo project, and it's also the foundation for team workflows later.

