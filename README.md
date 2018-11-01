I use this page to store and organize what I learned about Git.  

* [Installation](https://github.com/K1-ZR/practice-git/blob/master/README.md#installation)  
* [Start](https://github.com/K1-ZR/practice-git/blob/master/README.md#start)  
* [Submitting a change](https://github.com/K1-ZR/practice-git/blob/master/README.md#submitting-a-change)  
* [Show changes](https://github.com/K1-ZR/practice-git/blob/master/README.md#show-changes)  
* [Undo changes](https://github.com/K1-ZR/practice-git/blob/master/README.md#undo-changes)  
* [Branch](https://github.com/K1-ZR/practice-git/blob/master/README.md#branch)  
* [Remote server](https://github.com/K1-ZR/practice-git/blob/master/README.md#remote-server)  
* [Tag](https://github.com/K1-ZR/practice-git/blob/master/README.md#tag)  
* [Bug search](https://github.com/K1-ZR/practice-git/blob/master/README.md#bug-search)
* [Examples](https://github.com/K1-ZR/practice-git/blob/master/README.md#examples)

# Installation
* install Git from [here](https://git-scm.com/).
# Start
* change directory
```shell
cd <repo_dir>
```
* initialize Git 
```shell
git init
```
* check git status, it is always useful
```shell
git status
```
* get help
```shell
git help [keyword]
#        blame
```
# Submit a change
Any changes in directory    &#x27F6;    Stage    &#x27F6;    Commit  
the **staged** changes means Git tracks them  
the **committed** changes means Git archives them
* Stage a change
```shell
git add <file name>
#       -A or --all 
#       all files
```
* commit a change
```shell
git commit -m '<message>'
```
# Show changes
* show previous commits
```shell
git log
```
* show the changes happened in a commit
```shell
git show <commit_hash_string>
```
* show difference between unstaged changes and HEAD. HEAD is the tip of the current branch.
```shell
git diff HEAD
```
* show the difference between staged changes and HEAD.
```shell
git diff HEAD --staged
```
* Show who made the changes
```shell
git blame <file_name> -L<line_number>
#                     -L<start_line_number, end_line_number>
```
# Undo changes
* to unstage the changes
```shell
git reset <file_name>
```
* Fetch the last commit of the file
```shell
git checkout -- <file_name>
```
# Branch
* create a branch. Git clones your current branch.
```shell
git branch <branch_name>
```
* list branches
```shell
git branch
```
* jump to a branch. Git fetches the last commit of the branch.
```shell
git checkout <branch_name>
```
* merge the branch to master
```shell
git checkout master
git merge <branch_name>
```
* delete a branch
```shell
git branch -d <branch_name>
```
# Remote server
remote server (usually called as origin)  
push &#x2191;  &#x2193; pull  
local directory
* clone an existing remote repo to your local directory
```shell
git clone <remote_URL>
# it makes a directory with the repo name
```
* add a remote server to your local directory
multiple remote can be added to a local directory
```shell
git remote add <remote_name> <remote_URL>
#               origin
```
* remove a remote
```shell
git remote rm <remote_name>
```
* get info about remote
```shell
git remote [-V]
```
* pull changes to a local directory
```shell
git pull origin master
# pull from origin remote, master branch
```
* push changes to a remote server
```shell
git push [--all | --mirror] origin master
# push to origin remote, master branch
# -- all : Push all branches (i.e. refs under refs/heads/)
# --mirror : all refs under refs/ (which includes but is not limited to refs/heads/, refs/remotes/, and refs/tags/)
```
# Tag
Its difference with the branch is that you cannot commit to a tag.
* setting a tag
```shell
git tag -a <tage_name> [commit_hash_string] -m '<message>'
# if commit hash is omitted, tag is set to the current commit 
```
* show tags
```shell
git tag
```
* show a specific tag
```shell
git show <tag_name>
```
* push a tag to remote
```shell
git push origin <tag_name>
```
* fetch a tag
```shell
git checkout <tag_name>
```
# Bug search
* in order to find the problematic commit, follow these:
it uses bisect (binary search commit)
```shell
# start the process
git bisect start
```
```shell
# tell Git that the current commit is bad
git bisect bad
```
```shell
# tell Git the good commit
git bisect good <commit_hash_string>
```
Git suggest the next commit that needs to be checked. This process will be repeated 
until Git narrows commits down to the problematic commit.  
# Examples  
## Publishing an existing repository on a remote server
* First, create a new repository on remote serve without README, .gitignore or License. 
```shell
# change into the `my-repo` directory
cd <repo_dir>

# initialize Git
git init 

# stage changes
git add <file_name>

# take a snapshot of the staging area
git commit -m '<message>'

# provide a remote server for it
git remote add origin <remote_address>

# push changes to the remote server
git push --set-upstream origin master
```
## Contribute to an existing repository
```shell
# download a repository on a remote server to your local machine
# this makes a repo dir in your local machine
git clone <remote address>

# change into the `repo` directory
cd repo

# create a new branch to store any new changes
git branch <branch_name>

# switch to that branch 
git checkout <branch_name>

# make changes

# stage the changed files
git add <file_name>

# commit changes
git commit -m '<message>'

# push changes to the remote server
git push --set-upstream origin <branch_name>
```
## Contribute to an existing branch on GitHub
```shell
# change into the `repo` directory
cd repo

# update all remote tracking branches, and the currently checked out branch
git pull

# change into the existing branch called `feature-a`
git checkout <branch_name>

# make changes

# stage the changed file
git add <file_name>

# take a snapshot of the staging area
git commit -m '<message>'

# push changes to the remote server
git push
```
