# Introduction to Git Merge Conflict

In order to understand the git merge concept, let perform this small lab.

## Step-01: Create a folder and Initialize it as Git repo

- Create a folder "C:\binapp" and open it in Visual Studio Code.
- Go to **View** menu >> **Terminal**.

- In the terminal, type following command to **initialize the folder as git repo**:

```
git init
```

- **Check if you have any default branches. If yes, you're on which branch?** :

```
git status

[The preceding command should say "On master branch" and "no commits"]
```

## Step-02: Add files to the Repo and Commit the changes

- Create a new file in the repo as **index.html**. Find below sample code for index.html:

```
# Sample code for index.html

<!DOCTYPE html>
<html>
<head>
  <title>Bindesh FoodApp</title>
  <style>
    body {
      background-color: #FFCC00;
    }
    h1 {
      font-family: "Comic Sans MS";
      text-align: center;
      padding-top: 140px;
      font-size: 60px;
      margin: -15px;
    }
    p {
      font-family: sans-serif;
	  font-size: 38px;
      color: #907400;
      text-align: center;
    }
  </style>
</head>
<body>
  <h1 id=ipText>Food is Awesome..!</h1>
  <p>Sample food ordering application</p>
</body>
</html>
```

- **Stage the changes**:

```
# To check the new changes in the repo
git status

# To stage the changes made to index.html
git add index.html
```

- Commit the changes

```
git commit -m "Added index.html page"
```

- Check all the commits

```
git log

OR

git log --oneline
```

## Step-03: (Optional) Rename the master branch to main

- **Rename the "master" branch to "main"**

```
git branch -M main

# Check if the branch name has been updated to main
git log
```

## Step-04: Create a new Feature branch (heading) & Commit new changes to it

```
# Create & checkout to new branch
git checkout -b heading-branch

[Make some changes in the header section of the index.html]

# After updating the application header logic, stage the changes
git add .

# Commit the heading changes
git commit -m "Header changes"

# List all the previous commits made on heading-branch
git log --oneline
```

## Step-05: Update index.html on main branch

- Switch to main branch

```
git checkout main
```

- Make some changes in the body section of the index.html and commit the changes

```
git add .

git commit -m "index page second update - heading"
```

## Step-06: Make changes to heading-branch

- Switch to heading-branch

```
git checkout heading-branch
```

- Update the same line what we updated in Step#5, on main branch and commit the changes

```
git add .

git commit -m "index - discount update - main"
```

- List all the commits made so far on heading branch

```
git log --oneline
```

## Step-07: Merge the `heading-branch` into `main` branch

- **Switch to main branch**

```
git checkout main
```

- Merge the heading-branch into main branch

```
git merge heading-branch

[THIS ACT WILL RESULT IN MEREGE CONFLICT ERROR]
```

## Step-08: Resolve the Merge Conflict

- Make sure you're on the main branch (you may check it by running _git branch_ or _git status_ command).
- Open the index.html file and you will get to see following options:

  1. **Accept Current Changes** (made to index.html on main branch)
  2. **Accept Incoming changes** (main to index.html on heading-branch)
  3. **Accept both the changes**

- In order to resolve the conflict, select either option and merge it into main branch.

## Tips & Tricks

- **List all the local branches**

```
git branch -v
```

- **List all the remote branches**

```
git branch -r
```
