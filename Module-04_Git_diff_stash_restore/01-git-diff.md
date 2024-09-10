# Git Diff

## 01. Overview

- **git diff** show changes between commits, commit and working tree, etc

## 02. Documentation

- https://git-scm.com/docs/git-diff

## 03. How `git diff` works?

### Step-3.1: Create a local git repo

### Step-3.2: Add files to the repo and commit (v1)

### Step-3.3: Modify a file and commit the changes (v2)

### Step-3.4: Show changes between v1 & v2 commits

```
git diff

[The preceding command will display the changes made between last commit (v1) and the current version (v2)]
```

- Tip: You may list all the commits you've made so far by running following command:

```
git log --oneline
```

### Step-3.5: (Optional) Show changes between any two random commits

```
git diff <commit-id-1> <commit-id-2>
```
