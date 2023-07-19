# Introduction to GitHub
>**Distributed**: Each developer has a copy of the whole repository on their local machine.
>**GitHub**: a web-based Git repository hosting service. GitHub provides free access to a Git server for public and private repository.
>**WARNING**: for real configuration and development work, you should use a secure and private Git server, and limit the people authorized to work on it.

`git clone {{repository's url}}`
--> Create a local copy of the repository.
`git push` 
--> Send our changes from the local copy to the remote repository
`git config --global credential.helper cache`
--> Enable credential helper
`git pull`
--> Retrieve new changes from the repository and instantly merge

# Using a Remote Repository
>Terms:
>>Internet-based Git hosting providers
>>Git server
>>Git's distributed nature 
>
>Alongside the local development branches like master, Git keeps copies of the commits that have been submitted to the remote repository and separate branches
>![[Git remote workflow.png]]
>When working with remotes the workflow for making changes has some extra steps:
>1. Modify -> Stage -> Commit local changes
>2. Fetch any new changes from the remote repo manually merge if necessary and only then will push our changes to the remote repo.
>GitHub provides a variety of ways to connect to a remote repository: **HTTP**, **HTTPS**, **SSH protocols** and **their corresponding URLs**. **HTTP** is generally used to allow read only access to a repo. **HTTPS and SSH** both provide methods of authenticating users so you can control who gets permission to push
>
>When we call a `git clone` to get a local copy of a remote repository, Git sets up that remote repository with the default **origin** name.

`git remote -v`
--> Show the configuration for remote repository.
`git remote show origin`
--> Show more information about the configuration of the remote repository.

>Whenever we're operating with remotes, GIT uses **remote branches** to keep copies of the data that's stored in the remote repository.

`git branch -r`
--> Look at the remote branches that our Git repo is currently tracking
`git status`
--> Check the status of our changes in remote branches as well
`git fetch`
--> Copies the commits done in the remote repository to the remote branches, so we can see what other people have committed, but not instantly merge.
`git merge origin/master`
--> Merge the remote master branch into our local master branch
`git checkout {{same name as the new branch in the remote repo}}`
--> Create a new branch that has the same name as the other than master branch of the remote repo and switch to this branch. Git automatically copied the contents of the remote branch into the local branch
`git remote update`
--> Get the contents of remote branches without automatically merging any contents into the local branches