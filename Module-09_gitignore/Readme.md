# Git Files

## 01. gitignore file - Overview

- Git sees every file in your working copy as one of three things:

  1. Tracked
  2. Untracked
  3. Ignored

- Ignored files are usually build artifacts and machine generated files that can be derived from your repository source or should otherwise not be committed.

- Some of the examples of ignored files are:

  - Dependency caches
  - Compiled code
  - Build output directories
  - Hidden system files
  - Personal IDE config files
  - Credential files
  - Sensitive variable files

- Ignored files are tracked in a special file named **.gitignore**.
- **.gitignore** file is checked in at the root of your repository.
- The .gitignore file must be edited and committed by hand when you have new files that you wish to ignore.
- **.gitignore files** contain patterns that are matched against file names in your repository to determine whether or not they should be ignored.

## 02. .gitignore File Patterns

## 03. Working with .gitignore file

### Step-01: Create a new .gitignore file

- Create a new **.gitignore** file in the root of the git repo.

### Step-02: Decide what to ignore by Git

- Open the .gitignore file using a text editor like VS Code.

- Here, we are just going to add two simple rules:
  1. Ignore any files with the **.log extension**.
  2. Ignore everything in any directory named **temp**.

### Step-03: Update the .gitignore file with required expressions

```
# ignore ALL .log files
*.log

# ignore ALL files in ANY directory named temp
temp/
```

- Now all .log files and anything in temp folders will be ignored by Git.

## 04. Sample .gitignore file templates

- https://github.com/github/gitignore
