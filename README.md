I use this page to archive and organize what I learned about Git.  

* [Installation](https://github.com/K1-ZR/practice-git/blob/master/README.md#installation)  
* [Start](https://github.com/K1-ZR/practice-git/blob/master/README.md#start)  
* [Submitting a change](https://github.com/K1-ZR/practice-git/blob/master/README.md#submitting-a-change)  
* [Show changes](https://github.com/K1-ZR/practice-git/blob/master/README.md#show-changes)  
* [Undo changes](https://github.com/K1-ZR/practice-git/blob/master/README.md#undo-changes)  
* [Branch](https://github.com/K1-ZR/practice-git/blob/master/README.md#branch)  
* [Remote server](https://github.com/K1-ZR/practice-git/blob/master/README.md#remote-server)  
* [Tag](https://github.com/K1-ZR/practice-git/blob/master/README.md#tag)  
* [Bug search](https://github.com/K1-ZR/practice-git/blob/master/README.md#bug-search)
* [Scenarios](https://github.com/K1-ZR/practice-git/blob/master/README.md#scenarios)

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
git diff HEAD <file_name>
```
* show the difference between staged changes and HEAD.
```shell
git diff HEAD --staged <file_name>
```
* if you want to see what you haven't git added yet
```shell
git diff <file_name>
```
* or if you want to see already added changes
```shell
git diff --cached <file_name>
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
* Undoes all commits after commit, preserving changes locally
```shell
git reset <commit_hash_string>
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
git branch [-a]
# -a: list local and remote branches
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

```shell
# Deletes all stale remote-tracking branches. 
# These stale branches have already been removed from the remote, 
# but are still locally available in "remotes/<name>".
git remote prune origin
```
# Remote server
remote server (usually called as origin)  
push &#x2191;  &#x2193; pull  
local directory
* clone an existing remote repo to your local directory
```shell
# makes a directory with the repo name
git clone <remote_URL>
```
* add a remote server to your local directory
multiple remote can be added to a local directory
```shell
git remote add <remote_name> <remote_URL>
#               origin       https...
```
* remove a remote
```shell
git remote rm <remote_name>
```
* get info about remote
```shell
git remote [-V]
```
* list branches on remote
```shell
git remote show <remote-name>
```
* pull changes to a local directory
a pull is a fetch and a merge.
```shell
git pull <remote-name> <remote_branch-name>:<local_branch_name>
#        origin        master 
#        origin        myBranch:myBranch (use the same name)
```
* push changes to a remote server
```shell
git push <remote-name> <local-branch-name>:<remote_branch_name>
#        origin        master 
#        origin        myBranch:myBranch (use the same name)
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

# Scenarios  

## Publishing an existing repository on a remote server
* Step 1: create a new repository on remote serve without README, .gitignore or License. Then:
```shell
cd <repo_dir>
git init 
git remote add origin <remote_address>
git add -A
git commit -m '<message>'
git push origin master
```
## Contribute to an existing repository
```shell
git clone <remote address>
cd <repo_dir>
git branch <branch_name>
git checkout <branch_name>
# make changes
git add -A
git commit -m '<message>'
git push --set-upstream origin <branch_name>
```
## Contribute to an existing branch on GitHub
```shell
cd <repo_dir>
git pull # update all remote branches
git checkout <branch_name>
# make changes
git add -A
git commit -m '<message>'
git push
```
## My work flow when I work on a branch
I have:  
   * a `local-repo` on my PC  
   * another `local-repo` on my laptop  
   * a `remote-repo`  

I always make sure that my `remote-repo` is updated.
when I start from one of my `local-repo`s, I follow these steps
```shell
# step 1: make a branch on remote-repo
# step 2: check if any changes are made in local-repo
git status
# step 3: update my local-branch
git remote update # to bring your remote refs up to date
git status -uno # whether the branch you are tracking is ahead, behind or has diverged
# step 5: bring the working remote-branch to my local-repo local-branch (use the same name for both branches)
git pull <remote> <remote-branch>:<local-branch>
# step 6: make changes
# step 7: stage
# step 8: commit
# step 9: push changes to the working branch
git push <remote> <local-branch>:<remote-branch>
# step 10: go to remote-repo, i.e. GitHub, working branch and send a pull-request
# step 11: accept the changes into remote-master
# step 12: then at local
git pull origin master
# step 13: remove local and remote branches
$ git push --delete <remote_name> <branch_name>
$ git branch -d <branch_name>
$ git remote prune origin
# Deletes all stale remote-tracking branches. These stale branches have already been removed from the remote repository referenced by <name>, but are still locally available in "remotes/<name>".
```
# Relevant
* to modify VSCode extnesions:
```cpp
"[git-commit]": {
    "editor.rulers": [
      72
    ]
  }
```
