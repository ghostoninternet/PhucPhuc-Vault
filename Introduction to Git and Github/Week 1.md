# Version Control System
Version Control System (VCS): Keeps track of the changes we made to our files
We can make edits to multiple files and treat that collection of edits as a single change, which is commonly knowns as a **Commit**


Git has a distributed architecture: every person contributing to a repository has full copy of the repository on their own development machine. Git official website: [git-scm.com](git-scm.com)

# Using Control System
`git config --global <<your email>>`
`git config --global <<your name>>`
>Tell git who we are. The `--global` flag is used to set this value for all git repositories.

`git init`
>Initialize an empty git repository in the current directory .git: the directory that contains the changes and the change history 
>Executing **git init** creates a **.git** subdirectory in the current working directory, which contains all of the necessary Git metadata for the new repository. This metadata includes subdirectories for objects, refs, and template files. A HEAD file is also created which points to the currently checked out commit.
>If you've already run **git init** on a project directory containing a **.git** subdirectory, you can safely run **git init** again on the same project directory. The operation is what we call _idempotent_; running it again doesn't override an existing **.git** configuration.
>The area outside the git directory is the working tree. The working tree is the current version of your project. This working tree will contain all the files that are currently tracked by Git and any new files that we haven't yet add. 

### Configure Git
>Git uses a username to associate commits with an identity. It does this by using the **git config** command. To set Git username use the following command:
```Git
git config --global user.name "Name"
```
>Replace **Name** with your name. Any future commits you push to GitHub from the command line will now be represented by this name. You can use **git config** to even change the name associated with your Git commits. This will only affect future commits and won't change the name used for past commits.
```
git config --global user.email "user@example.com"
```
>Replace  **user@example. com** with your email-id. Any future commits you now push to GitHub will be associated with this email address. You can even use **git config** to change the user email associated with your Git commits.

### Git Operations
```Git
git add <<your file>>
```
>Add the file to the project so Git can track the file (Add the file to the staging area)
>Staging area (index): A file maintained by Git that contains all of the information about what files and changes are going to go into your next commit.

```Git
git status
```
>Get information about the current working tree and pending changes.

```
git commit
```
>Commit our changes, our new added files.


>Git tracks the changes and displays that the file has been modified. You can view the changes made to file using the following command:
```
git diff <<Your file>>
```
>You can see the differences between the older file and the new file. New additions are denoted by green-colored text and a + sign at the start of the line. Any replacements/removal are denoted by text in red-colored text and a - sign at the start of the line.

Each track file can be in one of three main states: 
1. modified: We have made changes to it but we haven't committed yet
2. staged: The changes to those files are ready to be committed to the project 
3. committed
```
git commit -m <<Message>>
```
> Commit the file and write the message at the same time. Can only be used with short message.

```
git config -l
```
>Check the current configuration

A commit message is generally broken up into a few sections:
1. The first line is a short summary of the commit followed by a blank line.
2. The next lines are a full description of the changes which details why they are necessary and anything that might be especially interesting about them or difficult to understand (<= 72 words)
