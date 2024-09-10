# Delete merged Git Branches

## 01. Delete merged Git Branch

- Here, I'm assuming that you've already merged the _heading-branch_ into **main** branch.

- Now, let's delete the heading-branch:

```
# Syntax
git branch -d <branch_name_to_be_deleted>

# Example
git branch -d heading-branch

# To verify the branch delete operation, simply list all the branches
git branch

[The preceding command shouldn't display heading-branch as it is deleted]
```
