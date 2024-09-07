# Git Restore

## 01. Overview

- **git restore** restore files to any previous commit.

## 02. Documentation

- https://git-scm.com/docs/git-restore

## 03. How `git restore` works?

### 3.1: Remove the changes you made since last commit (from working directory)

- I'm assuming that you have already committed changes one time (v1).

- Now, make some changes to any of your file.

- Check the changes you made since last commit (v1)

```
# To check the changes you made since last commit (v1)
git diff

# To check the git repo status
git status

[The preceding command will say a file in "modified" state]
```

- Remove the changes:

```
git restore <filename>

# Example
git restore index.html

[The preceding command will delete all the changes you made since last commit (v1)]
```

### 3.2: Unstage changes

- Modify a file present in your git repo and stage it:

```
# Stage the changes
git add <modified_file_name>

# Example
git add index.html
```

- Now, to unstage the changes and bring it back to working directory:

```
git restore --staged index.html

# To check the file changes status
git status

[The preceding command should show the file in working directory]
```

- To remove the changes you made since last commit (v1) from your working directory:

```
git restore <file_name>

# Example
git restore index.html
```
