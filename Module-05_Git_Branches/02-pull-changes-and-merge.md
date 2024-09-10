# Git - Pull new changes from the GitHub

-The **git pull** command is used to fetch and download content from a remote repository and immediately update the local repository to match that content.

- Merging remote upstream changes into your local repository is a common task in Git-based collaboration work flows.
- The **git pull** command is actually a combination of two other commands, **git fetch** followed by **git merge**.

## How git pull works?

- The git pull command first runs git fetch which downloads content from the specified remote repository.
- Then a git merge is executed to merge the remote content refs and heads into a new local merge commit.

## Common Options

```
# Fetch the specified remote branch copy of the current branch and merge it into the local copy
git pull <remote>

# Fetches the remote content but does not create a new merge commit
git pull --no-commit <remote>

# Same as the previous pull | Instead of using git merge, it use git rebase
# git pull --rebase <remote>
```

## References

- https://git-scm.com/docs/git-pull/en
