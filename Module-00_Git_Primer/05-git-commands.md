# Git - Best Practices

### 01. git version

- To view current installed version of the Git

  ```
  git --version
  ```

### 02. git init

- Initialize the directory as Git repository

  ```
  # Initialize a directory with no default branches
  git init
  ```

- Initialize a directory with a default branch

  ```
  git init --initial-branch=<BRANCH_NAME>

  OR

  git init -b <BRANCH_NAME>

  ```

### 03. git status

- Show the working tree status (branch)

  ```
  git status
  ```

### 04. git add

- Stage new changes.

```
# Adding selected files to the staging area
git add <FILE-01> <FILE-02> <FILE-N>

 OR

# Adding all the files from the working directory to staging area ( you may use -A --all)
git add .
```

### 05. git commit

### 06. git branch

- This command lets you create isolated development environments within a single repository.

- List all the branches in your repository

  ```
  git branch
  ```

- Create a new branch called ＜ BRANCH ＞. This does not check out the new branch

  ```
  git branch <BRANCH>
  ```

- Delete the specified branch and prevents you from deleting the branch if it has unmerged changes

  ```
  git branch -d <BRANCH>
  ```

- Force delete the specified branch, even if it has unmerged changes

  ```
  git branch -D <BRANCH>
  ```

- Rename the current branch to ＜ NEW-BRANCH-NAME ＞

  ```
  git branch -m <NEW-BRANCH-NAME>
  ```

- List all remote branches

  ```
  git branch -a
  ```

### 07. git log

- The git log command displays all of the commits in a repository's database on a particular branch.

  ```
  git log
  ```

### 08. git checkout

### 09. git clone

### 10. git config

### 11. git diff

### 12. git clean

### 13. git revert

### 14. git revert

### 15. git merge

```

```
