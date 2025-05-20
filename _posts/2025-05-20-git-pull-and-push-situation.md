What if you have to pull and push at the same time?
You can use below:

```bash
git fetch
git merge origin/your-branch
```

This is a **manual equivalent of `git pull`**, and it gives you more control over the process.

---

## 🔹 1. `git fetch`

```bash
git fetch
```

* This **downloads** the latest changes from the remote repository (e.g. GitHub or Bitbucket).
* It updates the special remote-tracking branches like `origin/main`, `origin/dev`, etc.
* **Important:** It does **not change your working directory** or local branches.

So after this:

* You have the latest info from the remote.
* But your local branch is still exactly as it was.

---

## 🔹 2. `git merge origin/your-branch`

```bash
git merge origin/your-branch
```

* This **merges the remote-tracking branch** (e.g. `origin/main`) into your current local branch (e.g. `main`).
* It applies the commits from the remote branch that are not in your local branch.
* If there are changes on both sides, this may create a **merge commit**.

---

## 🔁 Full Example:

Let’s say:

* Remote `main` has:

  ```
  A -- B -- C (origin/main)
  ```

* Your local `main` has:

  ```
  A -- D -- E (your commits)
  ```

### You run:

```bash
git fetch
git merge origin/main
```

Git will do:

```
      B -- C (origin/main)
     /
A -- D -- E -- M (merge commit)
```

Where **M** is a new commit combining both histories.

---

## ✅ Why use this over `git pull`?

* You get a **safe preview**: `git fetch` shows you what changed without touching your files.
* You can inspect with `git log origin/main` or `git diff HEAD origin/main` before merging.
* You avoid surprises — useful in automation or scripting.

---

## 🧠 Tip

If you want to avoid merge commits and keep history linear:

```bash
git fetch
git rebase origin/your-branch
```

Let me know your goal (e.g. clean history, automation, resolving conflicts) and I can suggest the best flow.
