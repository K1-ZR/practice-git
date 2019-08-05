I use this page for archiving what I learned about Git,
 mostly from [Pro Git](https://git-scm.com/book/en/v2). 

- [Install Git](#install-git)
- [Basics](#basics)
  - [File status](#file-status)
  - [Help](#help)
- [Config](#config)
- [Initialization](#initialization)
  - [Initializing a repository in an existing directory](#initializing-a-repository-in-an-existing-directory)
  - [Cloning a repository from an existing directory](#cloning-a-repository-from-an-existing-directory)
- [Recording Changes](#recording-changes)
  - [Track new files changes](#track-new-files-changes)
  - [Stage modified files](#stage-modified-files)
  - [Ignoring Files](#ignoring-files)
  - [Commit](#commit)
- [Viewing Changes](#viewing-changes)
  - [Difference between unstaged (working directory) vs staged](#difference-between-unstaged-working-directory-vs-staged)
  - [Difference between staged vs committed](#difference-between-staged-vs-committed)
- [Viewing the Commit History](#viewing-the-commit-history)
- [Undoing changes](#undoing-changes)
- [Remotes](#remotes)
  - [Fetching and pulling from remotes](#fetching-and-pulling-from-remotes)
- [Tagging](#tagging)
- [Branch](#branch)
- [Remote server](#remote-server)
- [Tag](#tag)
  - [Detached HEAD](#detached-head)
- [Bug Fix](#bug-fix)
  - [Bug search](#bug-search)
  - [Correcting bugs](#correcting-bugs)
<!-- --------------------------------------------------------------- -->
# Install Git
Install Git from [git-scm.com](https://git-scm.com/).

<!-- --------------------------------------------------------------- -->
# Basics
Git store data as a series of snapshots of directory.

- **Working Directory**: a single checkout of one version of the project.
- **Staged Files**: where Git stores will go into your next commit.
- **Committed Files**: where Git stores the snapshots of the project. 
  <!-- FIGURE 1 -->
<img src="images/filestate.png" width="50%">.

When one **stages**, Git
- computes a checksum for each file (the SHA-1 hash)
- stores that version of the file in the Git repository (Git refers to them as blobs)

When one **commits**, Git stores a commit object that contains:
- a pointer to the snapshot of the content you staged.
- author’s name and email
- commit message
- pointers to the commit or commits that directly came before this commit (its parent or parents): 
    - zero parents for the initial commit
    - one parent for a normal commit
    - multiple parents for a commit that results from a merge of two or more branches
  
In general, git repository has the following objects:
- blobs (each representing the contents of files)
- a tree that lists the contents of the directory and specifies which file names are stored as which blobs
- commit with the pointer to that root tree and all the commit metadata

## File status
Check files status:
```git
git status
```
## Help
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
# Initialization

## Initializing a repository in an existing directory
Change directory:
```git
cd <repository-directory>
```
Initialize Git:
```git
git init
```
## Cloning a repository from an existing directory
Clone from a remote directory:
```git
git clone <url>
```
<!-- --------------------------------------------------------------- -->
# Recording Changes

## Track new files changes
```git
git add <file_name>
```
## Stage modified files
```git
git add <file_name>
```
## Ignoring Files
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
## Commit  
Commit changes
```git
git commit -m "<my_message>"
```
<!-- --------------------------------------------------------------- -->
# Viewing Changes
## Difference between unstaged (working directory) vs staged
```git
git diff [<file_name>]
```
## Difference between staged vs committed
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
```git
git show <commit_hash>
```

Show who made the changes:
```git
git blame <file_name> -L<line_number>
```
<!-- --------------------------------------------------------------- -->
# Undoing changes
Unstage a staged file:
```git
git reset HEAD <file_name>
```

Revert the *working directory* back to the last committed
```git
git checkout -- <file_name>
```
<!-- --------------------------------------------------------------- -->
# Remotes
Lists each remote: 
```git
git remote [options]
```
Options:
- `-v`: shows remote URLs

Show lists the remote URLs and the tracking branch:
```git
git remote show <remote>
```

Add a new remote explicitly:
```git
git remote add <remote_name> <url>
```
## Fetching and pulling from remotes
Downloads all branches from the remote (just fetching without merging):
```git
git fetch <remote_name>
```
If your current branch is set up to track a remote branch, you can use the `git pull` to automatically fetch and then merge that remote branch into your current branch. 

By default, `git clone` sets up your local master branch to track the remote master branch.

<!-- --------------------------------------------------------------- -->
# Tagging
List tags:
```git
git tag
```

Annotate tag:
```git
git tag -a <tag_name> -m "<message>"
```

Tag a commit:
```git
git tag -a <tag_name> <commit_hash>
```

Show the tag data:
```git
git show <tag_name>
```

Push tag to a remote server
```git
git push <remote_name> <tag_name>.
```

Checkout a tag:
```git 
git checkout <tag_name>
```
<!-- --------------------------------------------------------------- -->
# Branch

A branch is a movable pointer to a commit. 
Every time a commit is made, the current branch pointer moves forward automatically.

When a new branch is created, Git creates a new pointer to move around. 

Git uses `HEAD` to know the current branch. 
`HEAD` is a pointer that always points to the current branch.

  <!-- FIGURE 2 -->
<img src="images/branching.png" width="80%">.

Create a branch (it just creates the branch without switching to that branch):
```git
git branch <branch_name>
```

Switching to a branch (Git moves HEAD to point to the that branch):
```git
git checkout <branch_name>
```

List branches:
```git
git branch [-a]
# -a: list local and remote branches
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
## Detached HEAD
In “detached HEAD” state, if you make changes and then create a commit, the tag will stay the same, but your new commit won’t belong to any branch and will be unreachable, except by the exact commit hash. Thus, if you need to make changes — say you’re fixing a bug on an older version, for instance — you will generally want to create a branch:
```git
git checkout -b <branch_name> <commit>
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
