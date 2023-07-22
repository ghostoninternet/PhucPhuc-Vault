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

# Code Reviews
>Going through someone else's code, documentation, or configuration and checking that it all makes sense and follows the expected patterns. 

>**Nit**: comments that are usually a suggestion for making the code better.

# Managing Projects
>If you're a project maintainer, it is important that you reply promptly to pull requests and don't let them stagnate.
>
>It's important that you understand any changes you accept.
>
>When it comes to coordinating who does what and when, a common strategy for active software projects is to use an **issue tracker**.

>In GitHub, each issue or pull request in a project has a unique number associated with it. So if we have a pull request with the ID five, there won't be an issue with ID five. GitHub will automatically reference issues and pull requests and comments when we mention them using the hash tag number format.

## Continuous integration system (CI)
>Will build and test our code every time there's a change.
>
>Once we have our code automatically built and tested, the next automation step is **continuous deployment**, which is sometimes called **continuous delivery (CD)**. --> New code is deployed often.
>
>Example tools: **Jenkins**, **GitLab**, **GitHub Actions**, **Travis (communicate with GitHub)**.
>
>Concepts:
>- **Pipeline**: Specify the steps that need to run to get the result you want.
>- **Artifacts**: The name used to describe any files that are generated as part of the pipeline.
>
>1. Make sure the authorized entities for the test servers are not the same entities authorized to deploy on the production servers
>
>2. Always have a plan to recover you access in case your pipeline gets compromised.
