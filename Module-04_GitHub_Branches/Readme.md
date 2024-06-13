# GitHub Branch

## 01. GitHub Workflow

- The main project is usually the production line, the version the end-users see and use.
- So, this version must be very clean and should always be usable.
- If errors are made in the _main_ version of your application, the clients or end-users might experience bugs, disrupting business.
- One way to resolve this issue is to create a copy of the _main_ project and work on this clone
- Each change you make to this copy does not affect the main project, so none of your mistakes can impact production app.
- And when you are perfectly sure that the changes you made resolve the issue, you can push those changes in the main version.
- These copies of the main project are called `branches`, and the concept of pushing changes into another branch is called `merging`.

- You can link your branch to the _main_ one so that reviewer can compare your changes and review the code. This link is called a `pull request`.

## 02. What are GitHub Branches?

- There is simple logic behind `branches`:
  1. Make a copy of the current state of the project.
  2. In this copy, you can make your changes without affecting other developer's work.
  3. You can use branches to implement new functionalities in parallel to your other project code versions (Production, Test, QA) without affecting it.
- You must work on your own branch before publishing your work so that you won't be worried by other people's changes.
- A branch is your independent copy of the project at a certain time.

## 03. Type of Branches

- **Main (Production) branch**
  - Main branch stores the official release history.
- **Develop (Integration) branch**
  - Develop branch serves as an integration branch for features
- **Release branch**

  - Once _develop_ branch has recieved enough features for a release, you create a _release_ branch off of develop branch.

- Creating develop branch starts the next release cycle, so no new features can be added after this point only, like:
  - Bug fixes
  - Documentation generation
  - Other release-oriented tasks
- Once it's ready to ship, the `release` branch gets merged into `main` branch and tagged with a version number.

- **Feature branch**

  - Each new feature should reside in its own branch, which can be pushed to the integration branch for collaboration.
  - Feature branches use **develop branch** as their **parent branch** instead of main branch.
  - When a feature is complete, it gets merged back into _develop_ branch.
  - Features should never interact directly with _main_ branch.

- **Hotfix (Maintenance) branch**
  - Hotfix branches are used to quickly patch production releases.
  - _Hotfix_ branches are somewhat similar to _Release_ branches and _Feature_ branches except they're based on _main_ branch instead of develop.
  - hostfix branch is the only branch that should fork directly off of main.
  - As soon as the fix is complete, it should be merged into main should be tagged with an updated version number.

## 04. Git Branching Strategies

- Gitflow
- GitHub-flow
- Trunk based Development

## 05. Working with GitHub Branches

### Step-5.1 Create a Branch

```
# Syntax
git branch <new_branch_name>

# Example
git branch develop
```

### Step-5.2 Switch to a particular branch

- List all the branches

  ```
  git branch
  ```

- Switch to a branch

  ```
  git checkout <branch_name>

  # Example
  git checkout develop
  ```

### 5.3 Merging a Branch

### 5.4 Deleting a Branch

### 5.5 Pushing a Branch to Remote repo
