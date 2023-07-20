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

# Solving Conflicts
>What if we try to push our commits from local repo to remote repo, while there are changes in the remote repo that our local repo hasn't had it ? --> It will failed and we will have to pull all new changes that exist in remote repo to our local remote repo, and then solve conflicts (if exist) and then push our changes to remote repo.

`git push -u origin {{branch's name}}`
--> ''-u' flag to create the branch upstream, which is another way of referring to remote repositories. 'origin' say that we want to push this to the origin repo, and that we are pushing the {{branch's name}} branch.
`git rebase {{branch's name that we want to set as the new base}}`
--> Rebase the branch that we are currently in. Git will try to replay our commits after the latest commit in that branch (the branch we rebase against). This will work automatically if the changes are made in different part of the files, but will require manual intervention if the changes were made in other files.
`git push --delete origin {{branch's name}}`
--> Remove remote branch
`git branch -d {{branch's name}}`
--> Remove local branch

>Always synchronize your branches before starting any work on your own
>Avoid having very large changes that modify a lot of different things.
>When working on a big change, it makes sense to have a separate feature branch. 
>Regularly merge changes made on the master branch back onto the feature branch
>Have the latest version of the project in the master branch, and the latest stable version of the project on a separate branch.
>You shouldn't rebase changes that have been pushed to remote repos.
>Having good commit messages is important