#

## GIT CHEAT SHEET

| Syntax | Description |
| ----------- | ----------- |
| Commit | an object |
| Branch | a reference to a commit; can have a tracked upstream |
| Tag | a reference (standard) or an object (annotated) |
| Head | a place where your working directory is now |

### Git Configuration

        git config --global user.name “Your Name”

Set the name that will be attached to your commits and tags.

        git config --global user.email “you@example.com”

Set the e-mail address that will be attached to your commits and tags.

        git config --global color.ui auto

Enable some colorization of Git output.

### create a new repository

        git init

Create a new local repository. If [project name] is provided, Git will create a new directory name [project name] and will initialize a repository inside it. If [project name] is not provided, then a new repository is initialized in the current directory.

### Checkout A Repository

create a working copy of a local repository by running the command

        git clone /path/to/repository

when using a remote server, your command will be

        git clone username@host:/path/to/repository

Downloads a project with the entire history from the remote repository.

### Workflow

your local repository consists of three "trees" maintained by git. the first one is your `Working Directory` which holds the actual files. the second one is the `Index` which acts as a staging area and finally the `HEAD` which points to the last commit you've made.

![alt text](http://rogerdudler.github.io/git-guide/img/trees.png)

### Add & Commit

        git status

Displays the status of your working directory. Options include new,
staged, and modified files. It will retrieve branch name, current commit
identifier, and changes pending commit.

You can propose changes (add it to the Index) using

        git add [filename]
        git add *
        git add .

> `add *` means add all files in the current directory, except for files whose name begin with a dot. This is your shell functionality and Git only ever receives a list of files.
> `add .` has no special meaning in your shell, and thus Git adds the entire directory recursively, which is almost the same, but including files whose names begin with a dot.

This is the first step in the basic git workflow. To actually commit these changes use.

        git commit -m "Commit message"

Now the file is committed to the HEAD, but not in your remote repository yet.

        git diff [file]

Show changes between working directory and staging area.

        git diff --staged [file]

Shows any changes between the staging area and the repository.

        git checkout -- [file]

Discard changes in working directory. This operation is unrecoverable.

        git reset [file]

Revert your repository to a previous known working state.

        git rm [file]

Remove file from working directory and staging area.

        git stash

Put current changes in your working directory into stash for later use.

        git stash pop

Apply stored stash content into working directory, and clear stash.

        git stash drop

Delete a specific stash from all your previous stashes.

### Pushing Changes

Your changes are now in the HEAD of your local working copy. To send those changes to your remote repository, execute

        git push origin master

Change master to whatever branch you want to push your changes to.

💡If you have not cloned an existing repository and want to connect your repository to a remote server, you need to add it with

        git remote add origin <server>

Now you are able to push your changes to the selected remote server.

### Branching

Branches are used to develop features isolated from each other. The master branch is the "default" branch when you create a repository. Use other branches for development and merge them back to the master branch upon completion.

        git branch [-a]

List all local branches in repository. With -a: show all branches (with remote).

        git branch [branch_name]

Create new branch, referencing the current HEAD.

        git checkout [-b][branch_name]

Switch working directory to the specified branch. With -b: Git will create the specified branch if it does not exist.

        git merge [from name]

Join specified [from name] branch into your current branch (the one you are on currently).

        git branch -d [name]

Remove selected branch, if it is already merged into any other. -D instead of -d forces deletion.

**Example:**

![alt text](http://rogerdudler.github.io/git-guide/img/branches.png)

Create a new branch named "feature_x" and switch to it using

        git checkout -b feature_x

switch back to master

        git checkout master

and delete the branch again

        git branch -d feature_x

a branch is not available to others unless you push the branch to your remote repository

        git push origin <branch>

### Update & Merge

To update your local repository to the newest commit, execute

        git pull

in your working directory to fetch and merge remote changes.

To merge another branch into your active branch (e.g. master), use

        git merge <branch>

in both cases git tries to auto-merge changes. Unfortunately, this is not always possible and results in conflicts. You are responsible to merge those conflicts manually by editing the files shown by git. After changing, you need to mark them as merged with

        git add <filename>

before merging changes, you can also preview them by using

        git diff <source_branch> <target_branch>

### Tagging

        git tag

List all tags.

        git tag [name] [commit sha]

Create a tag reference named name for current commit. Add commit
sha to tag a specific commit instead of current one.

        git tag -a [name] [commit sha]

Create a tag object named name for current commit.

        git tag -d [name]

Remove a tag from local repository.

**Example:**
it's recommended to create tags for software releases. this is a known concept, which also exists in SVN. You can create a new tag named 1.0.0 by executing

        git tag 1.0.0 1b2e1d63ff

the 1b2e1d63ff stands for the first 10 characters of the commit id you want to reference with your tag. You can get the commit id by looking at the...

### Log

        git log

in its simplest form, to study repository history

        git log [-n count]

List commit history of current branch. -n count limits list to last n commits.

        git log --author=bob

You can add a lot of parameters to make the log look like what you want.
To see only the commits of a certain author:

        git log --pretty=oneline

To see a very compressed log where each commit is one line:

        git log --graph --oneline --decorate --all

Or maybe you want to see an ASCII art tree of all the branches, decorated with the names of tags and branches:

        git log --name-status

See only which files have changed:

        git log ref..

List commits that are present on the current branch and not merged
into ref. A ref can be a branch name or a tag name.

        git log ..ref

List commit that are present on ref and not merged into current
branch.

        git reflog

List operations (e.g. checkouts or commits) made on local repository.

        git log --help

These are just a few of the possible parameters you can use. For more, see ⬆️

### Replace Local Changes

In case you did something wrong, which for sure never happens ;), you can replace local changes using the command

        git checkout -- <filename>

This replaces the changes in your working tree with the last content in HEAD. Changes already added to the index, as well as new files, will be kept.

Switches the current branch to the target reference, leaving
a difference as an uncommitted change. When --hard is used,
all changes are discarded.

        git reset [--hard] [target reference]

If you instead want to drop all your local changes and commits, fetch the latest history from the server and point your local master branch at it like this

        git fetch origin
        git reset --hard origin/master

Create a new commit, reverting changes from the specified commit. It generates an inversion of changes.

        git revert [commit sha]

## Synchronizing Repositories

        git fetch [remote]

Fetch changes from the remote, but not update tracking branches.

        git fetch --prune [remote]

Delete remote Refs that were removed from the remote repository.

        git pull [remote]

Fetch changes from the remote and merge current branch with its
upstream.

        git push [--tags] [remote]

Push local changes to the remote. Use --tags to push tags.

        git push -u [remote] [branch]

Push local branch to remote repository. Set its copy as an upstream.

### Git Fetch Origin

built-in git GUI

        gitk

use colorful git output

        git config color.ui true

show log on just one line per commit

        git config format.pretty oneline

use interactive adding

        git add -i

delete specific file on git and filesystem

        git rm file1
        rm 'file1'

create new file on filesystem
        vi filename.ext

remove a file into branch but not into filesystem
        git rm --cached filename.ext
        
                        