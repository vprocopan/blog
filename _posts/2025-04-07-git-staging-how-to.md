Using **staging in Git** means you're preparing changes for a commit. Here's a breakdown of how it works and the commands you use:

---

### 🧠 What is the **Staging Area**?
Think of the **staging area** (also called the **index**) as a buffer between your working directory and your project history (commits). It lets you group and review changes before saving them permanently with a commit.

---

### 🛠️ Common Git Staging Commands

#### 1. **Check what’s changed**
```bash
git status
```
- Shows which files are **modified**, **new**, or **deleted**, and whether they are staged or not.

#### 2. **Stage a specific file**
```bash
git add <filename>
```
- Adds the changes in that file to the staging area.

#### 3. **Stage all changed files**
```bash
git add .
```
- Stages everything in the current directory and below.

#### 4. **Stage part of a file (interactive)**
```bash
git add -p
```
- Lets you stage specific *hunks* (sections) of a file. Great for committing logically separated changes.

#### 5. **Unstage a file**
```bash
git restore --staged <filename>
```
- Removes the file from the staging area (but keeps the changes in your working directory).

#### 6. **View what's staged**
```bash
git diff --staged
```
- Shows the differences between the **staged** files and the **last commit**.

#### 7. **Commit staged changes**
```bash
git commit -m "Your commit message"
```

---

### 🧪 Example Workflow
```bash
# 1. Make changes to files
# 2. Check status
git status

# 3. Stage a file
git add app.js

# 4. Confirm what's staged
git diff --staged

# 5. Commit the staged file
git commit -m "Add new feature to app.js"
```

---

Want a visual Git staging tool too? Tools like **GitKraken**, **SourceTree**, or `git gui` can help.
