
To undo a Git merge on your local repository, the approach depends on the current state of the merge:

---

### 🛑 Scenario 1: Merge in Progress (Uncommitted)

If you've initiated a merge but haven't committed yet, and want to cancel it:

```bash
git merge --abort
```



This command will stop the merge process and revert your working directory to the state before the merge began.([Git Scripts][1])

---

### ✅ Scenario 2: Merge Committed Locally (Not Pushed)

If you've completed the merge and committed it locally but haven't pushed to the remote repository:

```bash
git reset --hard HEAD~1
```



This command resets your current branch to the commit before the merge, effectively removing the merge commit and any changes introduced by it. Be cautious: this will discard any uncommitted changes in your working directory.([GeeksforGeeks][2])

---

### 🔁 Scenario 3: Merge Committed and Pushed to Remote

If the merge has been committed and pushed to the remote repository, it's safer to create a new commit that undoes the changes introduced by the merge:([Warp][3])

```bash
git revert -m 1 <merge_commit_hash>
```



* Replace `<merge_commit_hash>` with the hash of the merge commit you want to revert.
* The `-m 1` option specifies that you want to keep the changes from the first parent of the merge (usually the branch you merged into).([Codedamn][4], [git-tower.com][5])

This approach preserves the history and is recommended when the merge has been shared with others.

---

### 🧭 Finding the Merge Commit Hash

To identify the hash of the merge commit you wish to undo:

```bash
git log --oneline
```



Look for the commit message associated with the merge to find its hash.

---

### ⚠️ Important Considerations

* **Avoid using `git reset` on public/shared branches** unless you're certain that no one else is working on them, as it rewrites history and can cause issues for collaborators.
* **Always ensure you have backups or have pushed your changes to a remote repository** before performing operations that alter commit history.

