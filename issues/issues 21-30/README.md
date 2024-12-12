Here are 10 additional common Git/GitHub problems with examples:

---

### **21. Confusing Remote Names**  
**Problem:** Using vague or default remote names like `origin` makes it hard to differentiate between repositories.  
**Example:**  
You add two remotes but don’t rename them:  
```bash
git remote add origin https://github.com/user/repo.git
git remote add origin https://github.com/user/fork.git
```
Commands like `git push origin main` fail because Git doesn’t know which remote you mean.  
**Solution:**  
Use descriptive names for remotes:  
```bash
git remote add upstream https://github.com/original/repo.git
git remote add myfork https://github.com/user/fork.git
```

---

### **22. Fork and Upstream Sync Issues**  
**Problem:** Keeping your fork in sync with the original repository can be tricky.  
**Example:**  
Your forked repository is behind the original:  
```bash
git fetch upstream
git merge upstream/main
```
Conflicts arise because your fork has diverging changes.  
**Solution:**  
Use rebase to sync forks:  
```bash
git fetch upstream
git rebase upstream/main
```

---

### **23. Tracking Too Many Branches**  
**Problem:** Managing many branches locally leads to confusion and errors.  
**Example:**  
You have old branches that are no longer needed:  
```bash
git branch -a
```
Shows:  
```bash
feature-1  
feature-2  
bugfix-1  
remotes/origin/feature-1  
remotes/origin/feature-3
```
**Solution:**  
Clean up unused branches:  
```bash
git branch -d feature-1
git fetch -p
```

---

### **24. Ignoring `.gitkeep`**  
**Problem:** Git doesn’t track empty directories, causing confusion.  
**Example:**  
You want to keep an empty `logs` directory:  
```bash
mkdir logs
git add logs
# Nothing to track.
```
**Solution:**  
Add a `.gitkeep` file in the directory:  
```bash
touch logs/.gitkeep
git add logs/.gitkeep
```

---

### **25. Mismatched Line Endings**  
**Problem:** Files edited on different operating systems cause line-ending issues.  
**Example:**  
On Windows, files use `CRLF`, but on Linux/Mac, they use `LF`.  
Git flags changes like:  
```diff
- Some code\r
+ Some code
```
**Solution:**  
Set `.gitattributes` to normalize line endings:  
```text
* text=auto
```

---

### **26. Large Commit Sizes**  
**Problem:** Committing too many changes at once makes it hard to track specific updates.  
**Example:**  
You add and commit all changes:  
```bash
git add .
git commit -m "Massive commit"
```
Later, debugging becomes a nightmare.  
**Solution:**  
Break commits into logical chunks:  
```bash
git add file1
git commit -m "Add feature X"
git add file2
git commit -m "Fix bug Y"
```

---

### **27. Overwritten Local Changes**  
**Problem:** Pulling updates overwrites your local work.  
**Example:**  
You modified a file but didn’t commit it:  
```bash
git pull origin main
```
Your changes are overwritten by the remote version.  
**Solution:**  
Stash your changes before pulling:  
```bash
git stash
git pull
git stash apply
```

---

### **28. Git Hook Errors**  
**Problem:** Pre-commit or post-commit hooks fail and block commits.  
**Example:**  
A pre-commit hook prevents your commit:  
```bash
git commit -m "Add feature"
# Error: Linting failed.
```
**Solution:**  
Fix the issue or temporarily skip the hook:  
```bash
git commit --no-verify -m "Skip hook"
```

---

### **29. Accidental Force Push to `main`**  
**Problem:** A force push overwrites commits on the `main` branch.  
**Example:**  
```bash
git push --force origin main
```
Your teammates’ work is lost.  
**Solution:**  
- Restore commits using `reflog`:  
  ```bash
  git reflog
  git reset --hard <commit-hash>
  ```
- Enable branch protection on GitHub to prevent force pushes.

---

### **30. Incorrect Default Branch**  
**Problem:** The wrong branch is set as the default, confusing new collaborators.  
**Example:**  
The default branch is `develop`, but most work happens on `main`.  
**Solution:**  
Change the default branch in GitHub settings:  
1. Go to *Settings > Branches*.  
2. Select the correct branch as the default.  
