# Version Control System (VCS)

## 01. What is Version Control?

- Version control involves managing multiple versions of a project code.

- It tracks every change made into project files (additions, edits, or deletions).

- Each change is recorded, which enables us to go back in time and see the change history.
- It also enables us to rollback to any previous version if required.
- To effectively implement version control, you need to utilize version control systems.
- These systems facilitate navigation through changes and provide a swift way to revert to previous versions when needed.

## 02. Key Advantages of Version Control

- History Tracking
- Collaboration among developers

## 03. Types of Version Control Systems

### 3.1 `Local` Version Control system

- These were the first VCSs created to manage source code.
- It tracks the changes made to files in a single database that is stored locally.
- All the change information is stored in the local database, locally on the system.
- If due to some reason system goes down, you lose the application code along with the change history info.
- **Example**: Source code control system (SCCS), Revision control system (RCS)

### 3.2 `Centralized` Version Control system

- Centralized VCS stores the change history on a single server to which the clients (authors) can connect.
- This offers a way to work with a team and allows monitoring a project's progress.
- The main problem with Central Version Control system is that a server error can result in losing all of the team’s work.
- A network connection is also required since the main project is stored on a remote server.

### 3.3 `Distributed` Version Control system

- Distributed VCS works similarly to centralized VCS but
  with a significant difference that no main server holds all the history.
- Instead, each client has a copy of the repository (including the change history) rather than checking out on a single server.
- This drastically reduces the risk of losing your code since
  each client has a clone of the project.
- With a distributed VCS, each client has all the power within their own repository.
