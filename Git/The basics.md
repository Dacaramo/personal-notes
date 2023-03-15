**Git**: Version Control System (VCS). In Git, every interaction that a development team can have with a ***codebase*** is recorded.

**Repository**: Project being tracked by ***Git***, already has a ***Git*** history.

**Commit**: Each of the changes registered in git.

**Branches**: New paths taken by the project, there is a main branch (master branch), it is the version in production. Every time you want to make changes or do something new, a new branch is generated. (it is an isolated environment) if something is damaged in this branch it will not compromise the operation of the project. If all goes well when the modification is finished, the branch is merged into the project.

**Clone**: It is an exact copy of the repository. When a programmer joins a working group, the first thing to do is clone the project.

**Fork**: It is a totally different project from the main project, which starts from the same version but does not have the objective of integrating or unifying the main one. Example: Linux distributions. They are based on other projects.

**Merge**: When a branch is approved and has no errors and works correctly, it is integrated (merged to the main branch).

The first thing to do to start working with git is to create or clone the repository: You create it using git init if it's going to be a brand new project or git clone if it's an existing repository.

The next thing is to commit your changes to the repository via commits, commits manually (as in Oracle SQL server).

Modifications can be temporarily saved in the staging area for later commit