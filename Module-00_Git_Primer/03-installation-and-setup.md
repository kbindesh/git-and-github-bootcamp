# Install and Configure `Git`

In this section, we will learn following topics:

_1. Install and Configure Git on Linux_

_2. Install and Configure Git on Windows_

_3. Setup your Git folder (Linux)_

## Reference

- Git official download page: https://www.git-scm.com/downloads

## 01. Install and Configure Git on Linux

### Step-1.1: Install Git

- Install Git on Amazon Linux 2/CentOS/RHEL

  ```
  sudo yum install -y git
  ```

- For installing Git on other linux distributions, kindly refer this link: https://www.git-scm.com/download/linux

### Step-1.2: Verify the Git installation

- On terminal, run following command to check if the git is installed on the system:

```
# Display the current installed version of the Git
git --version
```

### Step-1.3: Configure Git

- To configure Git, you must define some global variables:
  1. **user.name**
  2. **user.email**
- Both are required for you to make commits.

```
# Replace <USER_NAME> with the user name
git config --global user.name "<USER_NAME>"

# Replace <USER_EMAIL> with your e-mail address
git config --global user.email "<USER_EMAIL>"
```

- You may run the following command to check all the git variables (including email and name):

```
git config --list

OR

git config -l

# You must see "user.name" and "user.email" variables updated with your details
```

## 02. Install and Configure Git on Windows

### Step-2.1: Download, Install and Configure Git

- Download the Git binary from https://git-scm.com/download/win
- Once the executable is download, click on it to launch the installation.

### Step-2.2 Verify the Git installation

- In order to check if the Git is properly installed navigate to **Start menu** >> **Command Prompt**

```
git --version

# Above command should give you current installed version of the Git; something like this
git version 2.41.0.windows.3
```

### Step-2.3: Configure Git

- To configure Git, you must define some global variables:
  1. **user.name**
  2. **user.email**
- Both are required for you to make commits.

```
# Replace <USER_NAME> with the user name
git config --global user.name "<USER_NAME>"

# Replace <USER_EMAIL> with your e-mail address
git config --global user.email "<USER_EMAIL>"
```

- You may run the following command to check all the git variables (including email and name):

```
git config --list

OR

git config -l

# You must see "user.name" and "user.email" variables updated with your details
```

## 03. Setup your Git Repository (Linux)

### Step-3.1: `Create a project` folder

- Git works by checking for changes to files within a certain _folder or a directory_.
- So let's create a folder to serve as our project directory and let Git know about it, so it can start tracking changes.
- Start by creating an empty folder for your project, and then initialize a Git repository inside it:

  ```
  # Create a folder
  mkdir <DIRECTORY_NAME>

  # Change the directory to our project directory
  cd <DIRECTORY_NAME>
  ```

### Step-3.2: `Initialize` the project folder as Git repo

- Now, initialize your new repository and set the name of the default branch to main:

```
git init --initial-branch=main

or

git init -b main
```

- After initializing the repository, you should see output similar to this example:</br></br>

```
Initialized empty Git repository in /home/<user>/repository_name/.git/ </br>
  Switched to a new branch 'main'
```

### Step-3.3 Check the `Git repository status`

- Now, use a `git status` command to show the status of the working tree:

```
git status
```
