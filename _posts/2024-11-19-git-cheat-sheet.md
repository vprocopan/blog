**Git Cheat Sheet**

**Git: configurations**

$ git config --global user.name "FirstName LastName"

$ git config --global user.email "your-email@email-provider.com"

$ git config --global color.ui true

$ git config --list

**Git: starting a repository**

$ git init

$ git status

**Git: staging files**

$ git add <file-name>

$ git add <file-name> <another-file-name> <yet-another-file-name>

$ git add .

$ git add --all

$ git add -A

$ git rm --cached <file-name>

$ git reset <file-name>

**Git: committing to a repository**

$ git commit -m "Add three files"

$ git reset --soft HEAD^

$ git commit --amend -m <enter your message>

**Git: pulling and pushing from and to repositories**

$ git remote add origin <link>

$ git push -u origin master

$ git clone <clone>

$ git pull

**Git: branching**

$ git branch

$ git branch <branch-name>

$ git checkout <branch-name>

$ git merge <branch-name>

$ git checkout -b <branch-name>