# Undoing changes with `git clean`

## 01. What is git clean?

- **git clean** command helps you remove untracked files and directories from your working directory.
- These untracked files are usually not being tracked by Git, meaning they are not part of your repository's version history.
- They might have generated during app development, such as build, temporary files, or files you decided not to commit.

## 02. `Why` to use `git clean` command?

- Over the period of time, a repository can accumulate many untracked files, leading to clutter and potential confusion.
- git clean can help you maintaining a tidy repository.
- git clean also helps you in making sure that you do not commit unwanted files.
- git clean helps you in improving the efficiency of your repository by eliminating the unnecessary excess files.

## 03. `How` to use `git clean`

NOTE: Since, git clean command permanently deletes files, caution is necessary.

- Here are some of the steps to use _git clean_ safely:

```
# Preview the changes (what will be deleted using -n or --dry-run options
git clean -n

# Once you've reviewed, cleanup "files" using the -f or --force option

# Cleanup "directories" using -fd option
git clean -fd

# Remove ignored files (those are listed in .gitignore)
git clean -fx
```

## 04. `git clean` Limitations

By default, git clean will not remove:

- The .gitignore files
- New directories (which are created recently)
- Existing committed files

## 05. Demo

```
git clean
[The preceding command will result in following error:
fatal: clean.requireForce defaults to true and neither -i, -n, nor -f given; refusing to clean]

# Create a new file index.txt file and some text to it

# Check the git status (untracked files)
git status

# Check what files will be deleted
git clean -n

# Delete the additional files (index.txt | untracked files)
git clean -f

# Check the git status to confirm if untracked got deleted
git status
[The preceding command will tell you if untracked file got deleted ]

# Delete the ignored files and directories
git clean -fdx
```
