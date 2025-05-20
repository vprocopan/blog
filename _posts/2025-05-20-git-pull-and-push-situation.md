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


Great — let’s walk through a **visual example** of `git rebase origin/main` with **real commands** step-by-step.

---

### 🧪 Scenario:

You and your teammate are both working on the `main` branch.

* Your teammate made a commit and pushed it.
* Meanwhile, you made a local commit.
* Now, the branches look like this:

```
Remote (origin/main):     A --- B --- C
Your local (main):        A --- B --- D
```

So your local `main` has diverged from `origin/main`.

---

### 🎯 Goal:

Bring your local commit (`D`) **on top of** `origin/main` (`A-B-C`) — using `git rebase`.

---

### 💻 Step-by-step commands:

#### 🔁 1. Fetch the latest from remote

```bash
git fetch origin
```

Now Git knows about `origin/main` and its state (including commit `C`).

---

#### 🔎 2. Check the difference (optional but recommended)

```bash
git log --oneline --graph --all
```

You’ll see something like:

```
* d34db33 (HEAD -> main) D: Your local commit
| * c0ffee (origin/main) C: Teammate's commit
|/
* bbbbbbb B: shared base
* aaaaaaa A: initial commit
```

---

#### 🔁 3. Rebase your work onto the latest origin/main

```bash
git rebase origin/main
```

This means:

> “Take my commit D, and apply it on top of origin/main (which ends at C).”

If no conflicts, you'll get:

```
A --- B --- C --- D′
```

Now your local branch is linear and up-to-date.

---

#### 🚀 4. Push your changes

```bash
git push --force-with-lease
```

Since you rewrote history (D → D′), a normal `git push` would be rejected.
`--force-with-lease` safely updates the branch only if the remote hasn’t changed since your fetch.

---

### 🧠 Summary:

| Step | Command                           | Purpose                              |
| ---- | --------------------------------- | ------------------------------------ |
| 1    | `git fetch origin`                | Download latest changes              |
| 2    | `git log --oneline --graph --all` | Visualize history                    |
| 3    | `git rebase origin/main`          | Replay your commits on top of remote |
| 4    | `git push --force-with-lease`     | Push updated history                 |

---


![pull-push-sametime](/assets/images/pull-push.png)
