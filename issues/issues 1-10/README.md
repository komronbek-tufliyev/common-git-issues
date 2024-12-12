### **1. Merge Conflicts**  
**Problem:** Two developers edited the same part of a file in different ways, and Git doesn’t know which one to keep.  
**Example:**  
Developer A changes line 10 in `file.txt` to:  
```text
color = "blue"
```
Developer B changes line 10 to:  
```text
color = "red"
```
When merging, Git shows a conflict:  
```text
<<<<<<< HEAD
color = "blue"
=======
color = "red"
>>>>>>> feature-branch
```
**Solution:**  
Edit the file to resolve the conflict (e.g., decide which color to keep or merge the changes):  
```text
color = "blue and red"
```
Then commit the resolved file.

---

### **2. Accidentally Committed Secrets**  
**Problem:** A sensitive file with an API key or password is committed to the repository.  
**Example:**  
`config.json` contains:  
```json
{
  "api_key": "my-secret-key"
}
```
You accidentally add it to Git and push it.  
**Solution:**  
- Remove the file and update `.gitignore`:  
  ```bash
  echo "config.json" >> .gitignore
  git rm --cached config.json
  git commit -m "Remove sensitive file"
  git push
  ```
- Use tools like [BFG Repo-Cleaner](https://rtyley.github.io/bfg-repo-cleaner/) to scrub the secret from history.

---

### **3. Large Files in Repo**  
**Problem:** Adding large files (e.g., videos or binaries) slows down Git operations.  
**Example:**  
You mistakenly add a 200MB video:  
```bash
git add video.mp4
git commit -m "Add large file"
```
Later, Git operations (clone, fetch) become slow.  
**Solution:**  
- Use Git LFS:  
  ```bash
  git lfs track "*.mp4"
  git add .gitattributes
  git add video.mp4
  git commit -m "Add video with LFS"
  ```
- Alternatively, remove the file from history using BFG Repo-Cleaner.

---

### **4. Detached HEAD State**  
**Problem:** You’re working on a commit, not a branch, so your changes might not be saved.  
**Example:**  
```bash
git checkout <commit-hash>
# You’re now in detached HEAD state.
```
If you make changes and switch branches, your work is lost.  
**Solution:**  
- Save your work by creating a branch:  
  ```bash
  git checkout -b my-new-branch
  ```

---

### **5. Rewriting History on Shared Branch**  
**Problem:** Rewriting commit history on a shared branch causes others’ work to disappear.  
**Example:**  
You amend a commit and force-push to the shared `main` branch:  
```bash
git commit --amend
git push --force
```
Now, your teammate’s work is overwritten.  
**Solution:**  
Avoid using `git push --force`. Use `--force-with-lease` instead:  
```bash
git push --force-with-lease
```

---

### **6. Unintended Deletion of Branches**  
**Problem:** Deleting a branch before changes are merged leads to data loss.  
**Example:**  
```bash
git branch -d feature-branch
```
If the branch wasn’t merged, the changes are gone.  
**Solution:**  
- Restore the branch using `reflog`:  
  ```bash
  git reflog
  git checkout -b feature-branch <commit-hash>
  ```

---

### **7. Incorrect File Permissions**  
**Problem:** File permission differences (e.g., executable vs non-executable) appear as changes in Git.  
**Example:**  
Git shows this change:  
```bash
mode change 100644 => 100755 script.sh
```
**Solution:**  
Tell Git to ignore permission changes:  
```bash
git config core.fileMode false
```

---

### **8. Forgetting to Pull Before Push**  
**Problem:** You push changes without pulling the latest updates, causing a rejected push.  
**Example:**  
```bash
git push origin main
# Error: Updates were rejected because the remote contains work that you do not have locally.
```
**Solution:**  
Pull before pushing:  
```bash
git pull origin main --rebase
git push origin main
```

---

### **9. Squashing or Losing Commits**  
**Problem:** Using commands like `git reset` or `git rebase` improperly can lose commits.  
**Example:**  
```bash
git reset --hard HEAD~2
# Commits are now lost.
```
**Solution:**  
Recover lost commits with `reflog`:  
```bash
git reflog
git checkout <lost-commit-hash>
```

---

### **10. Outdated or Misconfigured `.gitignore`**  
**Problem:** Files that should be ignored are accidentally tracked.  
**Example:**  
You forget to add `.env` to `.gitignore` and commit it:  
```bash
git add .env
git commit -m "Add sensitive file"
```
**Solution:**  
Update `.gitignore`:  
```bash
echo ".env" >> .gitignore
git rm --cached .env
git commit -m "Remove .env from tracking"
```