# Advanced Git Interaction
`git commit -a`
--> Automatically stages every file that's **tracked** and modified before doing the commit letting it skip the `git add` step. `git commit -a` doesn't work on new files because those are **untracked**. You can't add any other changes before creating the commit.
**GIT** uses the **HEAD** alias to represent the currently checked-out snapshot of your project. This let you know what the content of your working directory should be. Think **HEAD** as a pointer to the current branch.

`git log`
--> Show the list of commits made in the current Git repository. 

`git log -p`
--> Look at the actual lines that changed in each commit.
`git show`
--> Takes a commit ID as a parameter and will display the information about the commit and the associated patch
`git log --stat`
--> Cause `git log` to show some stats about the changes in the commit.
`git diff`
--> Show us all the unstaged changes that we have made to that file. We can add a specific file as a parameter if the current Git repo contains many files. 
`git diff --staged`
--> Show all the changes that have been staged but not committed. 
`git add -p`
--> Git show us  the changes being added and ask us if we want to stage it or not.