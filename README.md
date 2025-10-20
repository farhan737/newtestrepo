# **GIT GUIDE**

## **1️⃣ Basic Git Workflow**

1. **Initialize a repo**

```bash
git init
```

2. **Check status**

```bash
git status
```

3. **Stage files**

```bash
git add <file>      # specific file
git add .           # all files
```

4. **Commit changes**

```bash
git commit -m "your message"
```

5. **Check history**

```bash
git log --oneline
git log --graph --decorate --all
```

6. **Push to remote**

```bash
git remote add origin <repo-url>   # first time
git push -u origin main
git push origin main               # subsequent pushes
```

---

## **2️⃣ Working with Branches**

* **Create branch**

```bash
git branch my-branch
```

* **Switch branch**

```bash
git switch my-branch
# or older style
git checkout my-branch
```

* **Create + switch**

```bash
git switch -c my-branch
```

* **Merge branch**

```bash
git checkout main
git merge my-branch
```

* **Delete branch**

```bash
git branch -d my-branch
```

* **Rename branch**

```bash
git branch -M main
```

**Tip:** Branches only exist after **at least one commit**.

---

## **3️⃣ Remote Repositories**

* **Add remote**

```bash
git remote add origin <repo-url>
```

* **List remotes**

```bash
git remote -v
```

* **Remove remote**

```bash
git remote remove origin
```

* **Fetch remote changes**

```bash
git fetch origin
```

* **Pull changes**

```bash
git pull origin main      # fetch + merge
git pull --rebase origin main   # fetch + reapply commits on top
```

* **Push changes**

```bash
git push origin main
git push origin main --force    # overwrite remote (use carefully)
```

---

## **4️⃣ Reset, Revert, and Undo**

* **Reset local branch to specific commit**

```bash
git reset --soft <commit>   # keep changes staged
git reset --mixed <commit>  # keep changes unstaged
git reset --hard <commit>   # discard changes, working dir matches commit
```

* **Reset to match remote**

```bash
git fetch origin
git reset --hard origin/main
```

* **Undo commit safely (without rewriting history)**

```bash
git revert <commit>         # creates new commit that undoes the changes
```

**Tip:** Use `reset --hard` only if you are sure about discarding changes, especially if remote has commits you don’t want to lose.

---

## **5️⃣ Understanding HEAD**

* `HEAD` → your **current branch or commit**
* `HEAD -> main` → local branch pointer
* `origin/HEAD` → remote default branch pointer
* Fetch or set `origin/HEAD`:

```bash
git fetch origin
git remote set-head origin -a
```

---

## **6️⃣ Common Situations & Pitfalls**

1. **Push rejected (non-fast-forward)**

   * Cause: Local branch is behind remote
   * Fix: `git pull --rebase` or `git push --force` (careful)

2. **Renamed or deleted files**

   * Git detects renames intelligently: `rename oldfile => newfile`
   * Must `git add` deletions/new files before commit

3. **Reset & pull**

   * Reset local branch: `git reset --hard <commit>`
   * Pull afterward will **fast-forward your branch** if remote is ahead
   * To **exactly match remote**, use `git reset --hard origin/main`

4. **Branches only exist after a commit**

   * If you `git init` and create a branch, it won’t show in `git branch` until first commit

---

## **7️⃣ Practical Example: Rollback Both Local & Remote**

```bash
# Step 1: Reset local branch to previous commit
git reset --hard <commit-to-keep>

# Step 2: Force push to remote
git push origin main --force
```

**Optional safe alternative** (avoids force push):

```bash
git revert <commit-to-undo>     # creates a new commit that undoes previous commit
git push origin main
```

---

## **8️⃣ Tips & Best Practices**

* Stage changes carefully (`git add`) before commit
* Check `git status` frequently
* Use `git log --oneline --graph --decorate --all` to visualize commits and branches
* Prefer `git revert` if the branch is shared
* Only use `--force` if you’re sure nobody else is working on the branch
