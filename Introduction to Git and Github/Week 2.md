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
--> Show us all the **unstaged changes** that we have made to that file. We can add a specific file as a parameter if the current Git repo contains many files. 
`git diff --staged`
--> Show all the **changes that have been staged** but not committed. 
`git add -p`
--> Git show us  the changes being added and ask us if we want to stage it or not.

`git rm {{file's name}}`
--> Remove files from your repository
File removals go through the same general workflow that we've seen, so you'll need to write a commit message as why you've deleted them.
`git mv {{file's old name}} {{file's new name}}`
--> Rename files in the repository. The `git mv` command works in a similar way to the mv command on Linux and so can be used for both moving and renaming.
>.gitignore
>Inside this file, we'll specify rules to tell git which files to skip for the current repo.

# Undoing Things
`git checkout {{file's name}}`
--> Change a file back to its earlier committed state
`git checkout -p {{file's name}}`
--> Checkout individual changes instead of a whole file. This will ask you change by change if you want to go back to the previous snapshot or not.
`git reset HEAD {{file's name}}`
--> Unstages the changes of a file.  Move all files out of the staging area.
`git reset -p`
--> Get git to ask you which specific changes you want to reset.
`git add *`
--> Add all changes in every files to the staging area.
`git commit --amend`
--> Git takes whatever is currently in our staging area and run the git commit workflow to overwrite the previous commit (allow us to modify and add changes to the most recent commit)
**DO NOT** use `git commit --amend` with public repository. Fixing up a local commit with amend is great but avoid amending commits that have already been made public
`git revert HEAD`
--> Creates a commit that contains the inverse of all the changes made in the bad commit in order to cancel them out. By using the **HEAD** alias, we can revert the latest commit. 
`git revert {{commit ID}}`
--> Revert to the commit that has the commit ID equal to the {{commit ID}}

# Branching and Merging
> **Branch**: A pointer to a particular commit. It represents an independent line of development in a project
> The default branch that git creates for you when a new repository is initialized is called **master**.
> We can use the `git branch` command to list, create, delete and manipulate branches

`git branch`
--> List all the branches in your repository
`git branch {{name of the new branch}}`
--> Create a new branch 
`git checkout`
--> Check out the latest snapshot for both files and for branches
`git checkout -b {{new branch's name}}`
--> Create a new branch and move to that branch

>When we switch branches, git also change files in our working directory or working tree whatever snapshot **HEAD** is pointing at.