# Push changes from a local machine to GitHub repo

## Step-01: Create a new GitHub Repository

- Sign-in to your GitHub Account >> https://github.com/ >> **Sign in**
- Select **Repositories** tab >> Click **New** button
- **Owner**: <your_username>
- **Repository Name**: <repo_name>
- **Description**: <repo_description>
- **Scope**: <public_or_private>
- **Initialize this repository with**: Enable
- Click **Create Repository** button

## Step-02: Integrate local repo with GitHub repo (Authentication)

There are basically three authentication methods avaiable with GitHub:

1. Token based authentication (secret token)
2. SSH based authentication (key)
3. HTTPS based authentication (username & password)

### Step-2.1: Token based Authentication | Generate a Token

- On your GitHub dashboard, towards left-top corner, click on user dropdown list >> **Settings**
- Now, from the leftside panel select **Developer settings** >> **Personal Access Tokens** >> **Fine-grained tokens** >> **Generate new token**

- **Token Name**: <any_label_for_token>
- **Expiration**: 30 days
- **Repository access**: All repositories
- **Update the permissions for all the actions**: Read and Write

- Click in **Generate token** button.
- Copy the generated token and store it at a safe place. We will need it shortly.

### Step-2.2: HTTPS based Authentication

### Step-2.3: SSH based Authentication

## Step-03: Create a local git repository

### Step-3.1: Create a folder

- Create a folder on your local system and open it in Command prompt:

```
cd <location_of_your_folder>

# Example
cd C:\GitTutorials
```

### Step-3.2: Initialize the folder as Git repo

```
git init
```

## Step-04: Configure where to push the code (here to GitHub repo)

```
git remote add <alias> <url_of_the_remote_git_repo>

# Example
git remote add origin https://github.com/kbindesh/github-bootcamp.git
```

## Step-05: Define the working Git Branch

- Set the default branch to commit the code:

```
git branch -M <branch_name>

# Example
git branch -M main

# To check the list of all the branches
git branch

$ To check on which branch has the control
git status
OR
git branch
```

## Step-06: Add files to the local repository

- Go to the folder and add few files to it.

## Step-07: Stage and Commit the changes (locally)

```
# Stage the changes made in one file
git add <filename>

# Stage the changes made in multiple files
git add .
```

- Commit the changes

```
git commit -m "commit_message_here"

# Example
git commit -m "App version 1.0"
```

## Step-08: Push the changes to GitHub repository

```
# Push the code to remote git repo | For alias, refer to Step#4
git push -u <remote_repo_alias> <branch_name>

# Example
git push -u origin main
```

## Step-09: Verify the GitHub repository for new changes
