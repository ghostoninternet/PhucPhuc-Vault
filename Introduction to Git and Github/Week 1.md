# Version Control System
Version Control System (VCS): Keeps track of the changes we made to our files
We can make edits to multiple files and treat that collection of edits as a single change, which is commonly knowns as a **Commit**


Git has a distributed architecture: every person contributing to a repository has full copy of the repository on their own development machine. Git official website: [git-scm.com](git-scm.com)

# Using Control System
`git config --global <<your email>>`
`git config --global <<your name>>`
--> Tell git who we are. The `--global` flag is used to set this value for all git repositories.
`git init`
--> Initialize an empty git repository in the current directory
.git: the directory that contains the changes and the change history
The area outside the git directory is the working tree. The working tree is the current version of your project. This working tree will contain all the files that are currently tracked by Git and any new files that we haven't yet add. 
`git add <<your file>>`
--> Add the file to the project so Git can track the file (Add the file to the staging area)
Staging area (index): A file maintained by Git that contains all of the information about what files and changes are going to go into your next commit.
`git status`
--> Get information about the current working tree and pending changes.
`git commit`
--> Commit our changes, our new added files.


Each track file can be in one of three main states: 
1. modified: We have made changes to it but we haven't committed yet
2. staged: The changes to those files are ready to be committed to the project 
3. committed
`git commit -m <<Message>>`
--> Commit the file and write the message at the same time.
`git config -l`
--> Check the current configuration


A commit message is generally broken up into a few sections:
1. The first line is a short summary of the commit followed by a blank line.
2. The next lines are a full description of the changes which details why they are necessary and anything that might be especially interesting about them or difficult to understand (<= 72 words)
