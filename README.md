I use this page for archiving what I learned about Git,
 mostly from [Pro Git](https://git-scm.com/book/en/v2). 

- [Basics](#basics)
      - [File status](#file-status)
      - [Help](#help)
- [Config](#config)
- [Install Git](#install-git)
- [Initialization](#initialization)
      - [Initializing a repository in an existing directory](#initializing-a-repository-in-an-existing-directory)
      - [Cloning a repository from an existing directory](#cloning-a-repository-from-an-existing-directory)
- [Recording Changes](#recording-changes)
      - [Track new files changes](#track-new-files-changes)
      - [Stage modified files](#stage-modified-files)
      - [Ignoring Files](#ignoring-files)
      - [Commit changes](#commit-changes)
- [Viewing Changes](#viewing-changes)
      - [Difference between unstaged (working directory) vs staged](#difference-between-unstaged-working-directory-vs-staged)
      - [Difference between staged vs committed](#difference-between-staged-vs-committed)
- [Viewing the Commit History](#viewing-the-commit-history)
- [Undo changes](#undo-changes)
- [Branch](#branch)
- [Remote server](#remote-server)
- [Tag](#tag)
- [Bug Fix](#bug-fix)
  - [Bug search](#bug-search)
  - [Correcting bugs](#correcting-bugs)
<!-- --------------------------------------------------------------- -->
# Basics

- **Working Directory**: a single checkout of one version of the project.
- **Staged Files**: where Git stores will go into your next commit.
- **Committed Files**: where Git stores the snapshots of the project. 
  <!-- FIGURE 1 -->
<img src="images/filestate.PNG", width=400 >

#### File status
Check files status:
```git
git status
```
#### Help
Get help about any keyword:
```git
git help <verb>
```
Get the more concise help:
```git
git <verb> -h
```
<!-- --------------------------------------------------------------- -->
# Config

Change username and email
```git
git config --global user.name "<my_name>"
git config --global user.email <my_email>
```
To override the global config, run it without `--global`.

<!-- --------------------------------------------------------------- -->
# Install Git
Install Git from [git-scm.com](https://git-scm.com/).

<!-- --------------------------------------------------------------- -->
# Initialization

#### Initializing a repository in an existing directory
Change directory:
```git
cd <repository-directory>
```
Initialize Git:
```git
git init
```
#### Cloning a repository from an existing directory
Clone from a remote directory:
```git
git clone <url>
```

<!-- --------------------------------------------------------------- -->
# Recording Changes

#### Track new files changes
```git
git add <file_name>
```
#### Stage modified files
```git
git add <file_name>
```
#### Ignoring Files
Create a `.gitignore` file in the repository directory similar to:
```git
# ignore all .a files
*.a
# but do track lib.a, even though .a are ignored
!lib.a
# only ignore the TODO file in the current directory, not subdir/TODO
/TODO
# ignore all files in any directory named build
build/
# ignore doc/notes.txt, but not doc/server/arch.txt
doc/*.txt
```
#### Commit changes
```git
git commit -m "<my_message>"
```
<!-- --------------------------------------------------------------- -->
# Viewing Changes
#### Difference between unstaged (working directory) vs staged
```git
git diff [<file_name>]
```
#### Difference between staged vs committed
```git
git diff --staged [<file_name>]
```
Use `git difftool` instead of `git diff` for more visual comparison.

**NOTE:** 
Git diff header is in the form of 
`@@ \<preimage-file-range> \<postimage-file-range> @@`.

where
- `file-range` is in the form `-\<start-line>,\<number-of-lines>`  
- `•`: The lines common to both files.
- `+`: A line was added here to the first file.
- `-`: A line was removed here from the first file.
<!-- --------------------------------------------------------------- -->
# Viewing the Commit History
List the previous commits:
```git
git log [option]
```
Options:
- `--patch`: shows the difference introduced in each commit
- `--oneline`: prints each commit on a single line
- `--oneline --graph`: adds a little ASCII graph showing your branch and merge history
- `--since "DEC 1 2014" --until "DEC 5 2014"`: shows commits between two dates
- `-S <a_string>`: show commits changing the number of occurrence of the string
  
Show the changes happened in a commit:
```shell
git show <commit_hash>
```

Show who made the changes:
```shell
git blame <file_name> -L<line_number>
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
* 
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
# the tag will refer to the commit of the branch you are currently on
# if commit hash is omitted, the tag will refer to the most recent commit
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
# Bug Fix
## Bug search
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

## Correcting bugs
* Checkout where the bug is added
```shell
git checkout <commit_hash_string>
```
Git automatically create a detached branch from that commit.
* Make changes
* commit changes
* Create a new branch where you are, then switch to master and merge it:
```shell
git branch my-temporary-work
git checkout master
git merge my-temporary-work
```
