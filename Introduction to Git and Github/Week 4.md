# Pull Request
>GitHub will automatically create a fork of the repository containing the file that we are editing but don't have access to. And if we submit changes to this file, it will create a new branch so that we can send a pull request.
> **FORKING**: A way of creating a copy of the given repository so that it belongs to our user
> **Typical workflow** when collaborating on GitHub: 
> 1. Create a fork of the repo
> 2. Work on that local fork
> 3. Merging our changes to the forking repo by creating a pull request
> **PULL REQUEST**: A commit or series of commits that you send to the owner of the repository so that they incorporate into their tree.
> **Typical Pull Request Workflow**: From the repo of an X user, create a fork for that repo. GitHub will automatically create a repository that has the same name, same files, same commits history as X user repo but this time is our repo. And then clone the forked repo to our local machine. 

`git rebase -i master`
--> An interactive version of the `rebase` command
`git push -f`
--> Force Git to push the current snapshot into the repo 