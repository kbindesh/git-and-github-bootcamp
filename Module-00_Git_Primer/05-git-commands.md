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

- The three stages of commit:

  1. Modify

  2. Stage

     ```
     # Stage all new changes
     git add .

     # Stage a particular file changes
     git add <FILE_NAME>
     ```

  3. Commit

     ```
     git commit -m "<COMMIT_MESSAGE>"
     ```

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

- A _checkout_ is the act of switching between different versions of a target entity.
- This commands lets you checking out old commits and old file revisions.
- This commands also lets you navigate to any existing branches.
  <br/><br/>
- Reviewing old commits and switch to any previous commit

  ```
  git log --oneline
  git commit <COMMIT_ID>
  ```

- Switching between the existing branches

  ```
  git checkout <EXISTING_BRANCH_NAME>
  ```

- Create a new branch and immediately switch to it

  ```
  git checkout -b <NEW_BRANCH_NAME>
  ```

- To checkout a remote branch you have to first fetch the contents of the branch, and then checkout

  ```
  git checkout -b ＜branchname＞
  git reset --hard origin/＜branchname＞
  ```

### 09. git clone

- _git clone_ is used to create a copy of a target repository.

  ```
  # Syntax
  git clone <GIT_REPO_URL>

  # Example
  git clone https://github.com/kbindesh/sample-repo.git
  ```

- Cloning to a specific folder

  ```
  git clone <REPO> <DIRECTORY>
  ```

- Cloning a specific tag

  ```
  git clone --branch <TAG> <REPO>
  ```

- Cloning a specific remote branch (-branch)

  ```
  git clone --branch <BRANCH_NAME>
  ```

### 10. git config

- git config command is used to set Git configuration values on a global or local project level.

<br/>

- Set git user email value

  ```
  git config user.email "sample_id@example.com"
  ```

- git config levels

  - --local
  - --global
  - --system

  ```
  # Set user email at "global" level
  git config --global user.email "your_email@example.com"

  # Set user email at "local" level
  git config --local user.email "your_email@example.com"

  # Set user email at "system" level
  git config --system user.email "your_email@example.com"
  ```

### 11. git show (view previous versions)

- To view and learn what changes have been made to your project, you can use the git show command followed by the name of the commit.
- You don’t need to write the full name; the first seven letters suffice.

  ```
  git show <NAME_OF_COMMIT>
  ```

### 12. git diff (reviewing current changes)

- _git diff_ command is used to examin differences between the last commit and the current
  working directory.

  ```
  git diff
  ```

- If you only need to check the changes made to a single file, not the entire project.

  ```
  git diff <FILE_NAME>
  ```

### 13. git revert

### 14. git revert

### 15. git merge
