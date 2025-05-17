---
layout: default
title:  Trucos Técnicos de Git
permalink: /git_cheatsheet/
parent: Git
has_children: false
has_toc: false
nav_order: 3
---

#  Trucos Técnicos de Git

{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>

---

## Git Directional Flow

This shows the typical flow git operations between a client and a server.
The direction of the flow indicates where the action is initiated and where it ends.

```sh
Operation  Direction       Description
---------- --------------  -------------------------------------------------------
git clone  server->client  initial repository download from remote repository
git pull   server->client  download latest updates from the remote repository
git push   client->server  upload or publish your changes to the remote repository
git add    client only     put files under revision control on the local workspace
git commit client only     register changes on the local workspace
```
A remote git repository can be accessed a number of ways depending on the location:

- git://example.com/proj/project.git
- https://example.com/proj/project.git
- ssh://user@example.com/proj/project.git

Each example uses a different protocol. 

{: .note }
Note the absence of HTTP. Avoid using insecure protocols.

## Create a Git Repository

This is the first task needed to implement revision control on a project.

A normal flow would be like this:

- User creates a project on their local machine.
- Go to the location where the project will live. This should be a location where the user has full access.
- Use the git client to initialize the git repository.

Example:

In this example the user creates the directory `projectX` in the `$HOME` location.
Then hey change directories to that location and use the git client with the init parameter to start implementing revision control.
```
-> cd /home/devuser/
-> mkdir projectX
-> cd projectX/
-> git init
```

{: .note }
At a later point the user may want to push the repository to an external location such as github.com. We explain this below.

## GIT Origin Operations

Adding an origin to a Git repository serves as a way to specify the default remote repository associated with your local repository. It simplifies collaboration and version control workflows by providing a convenient reference point for pushing changes to and pulling updates from a shared remote location.
The origin acts as a nickname (alias) for the remote repository URL, typically hosted on platforms like GitHub, GitLab, or Bitbucket. This makes managing remote connections easier.

### Add origin

Setup the client to send its commits to the remote git server.
Provide the git server hostname or ip address.
In order to accomplish this we must specify remote origin url [^1]

[^1]: See [add origin vs remote set-url origin](https://stackoverflow.com/questions/42830557/git-remote-add-origin-vs-remote-set-url-origin) on stackoverflow

Git command help to [manage remotes](https://git-scm.com/docs/git-remote#Documentation/git-remote.txt-remove).
```
NAME
       git-remote - manage set of tracked repositories
SYNOPSIS
       git remote [-v | --verbose]
       git remote add [-t <branch>] [-m <main>] [-f] [--mirror] <name> <url>
       git remote rename <old> <new>
       git remote rm <name>
       git remote set-head <name> (-a | -d | <branch>)
       git remote set-url [--push] <name> <newurl> [<oldurl>]
       git remote set-url --add [--push] <name> <newurl>
       git remote set-url --delete [--push] <name> <url>
       git remote [-v | --verbose] show [-n] <name>
       git remote prune [-n | --dry-run] <name>
       git remote [-v | --verbose] update [-p | --prune] [group | remote]...
```

**:: Using add**

Use `add` to add new remote
```
-> cd projectX
-> git remote add origin git@<git-server>:<gituser>/project.git
```

**:: Using set-url**

Use `set-url` to change (or replace) the url of an existing remote repository

Change origin to use SSH
```
-> git remote set-url origin git@github.com:lungithub/gitrepo1.git
```
Change origin to use HTTPS
```
-> git remote set-url origin https://github.com/lungithub/gitrepo1.git
```
Check the origin settings.
```
-> git config --get remote.origin.url
```
The same shows up in the git config file in the `[remote]` section.
```
[remote "origin"]
    url = git@172.16.15.199:project.git
    fetch = +refs/heads/*:refs/remotes/origin/*
```

Se the list available origins
```
-> git remote
-> git remote -v
```

## GIT Push

After having made changes to you project, the goal is to push them to a remote location.

First, you must add the changes to the local branches
```
-> git add .
```
Then you can compare with the main branch
```
-> git diff --stat origin/main
```
Send the changes to the git server.
You must be in the project directory to push changes.
```
-> git push origin main
```

### Remove origin

Steps to delete a remote [^2].

[^2]: Learn more about [removing-a-remote](https://help.github.com/articles/removing-a-remote/) on stackoverflow.

{: .important }
At any time, you can add a remote. It can be the same one you deleted or another one.

Syntax:
```
git remote rm destination
```
Example:
```
-> git remote rm origin   
```

### Check the remote where we are pushing data
```
-> git remote show origin
```


## Ignoring Files in Git

On **MACOS X** ignore `.DS_Store` hidden directories [^3].

[^3]: See this stackoverflow post about [ignoring .DS_Store]( http://stackoverflow.com/questions/18393498/gitignore-all-the-ds-store-files-in-every-folder-and-subfolder) on in every folder and subfolder

This removes all `.DS_Store` from a directory. You can do this on your project's diretory.
```
->$ find . -name .DS_Store -print0 | xargs -0 git rm --ignore-unmatch??
```
Create a global `.gitignore`
```
-> echo ".DS_Store" > /Users/devuser/.gitignore
```
Configure git to use the glogal `.gitignore`
```
-> git config --global core.excludesfile /Users/devuser/.gitignore
```

Add contents to the .gitignore.
Note that you can add comments using `#`.
```
-> cat .gitignore
.DS_Store
*.iso
*.log
*.tar.gz
*.tar
.*
*.[oa]
*~
# Ignore Chef key files and secrets
.chef/*.pem
.chef/encrypted_data_bag_secret
```
We don't want to push ISO, LOG, TAR or GZ files.
Add regex patterns as needed.
The entry '.*' ignores hidden files such as .bashrc.

## GIT File Operations

Create a file and put it under revision control.
```
-> cd /home/devuser/projectX
-> vi file.txt
-> git add file.txt
-> git commit -m"Added new file" file.txt
```
Say that you have made a bunch of changes to a file, you can  forget the
changes just made. Go back to the newest version on record.
```
-> git checkout -- file.txt
```
Rename a file
```
-> git mv <oldname> <newname>
```
A file can be removed from the repository in two ways:

[a] from the repository only; the file remains in the local filesystem
```
-> git rm --cache <file>
```
[b] from the repository and the local file system
```
-> git rm <file>
```

## Clone Git Repository

This section is just for reference. We should use a Github PAT for all git client operations.

[^4]: Learn about Github [Manage Personal Access Tokens](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens)

{: .warning }
It is highy encouraged to use Github PATs for git client operations [^4].

(a) Clone using SSH protocol

If there were a project named `projectX` on the git server, then clone with the command below.
```
-> git clone git@git-server:/home/devuser/projectX
-> git clone git@192.168.65.139:bye.git
```
(b) Clone using HTTPS protocol
```
-> git clone https://github.com//mygitrepo.git
```

## What is the Difference on a Git repository?

At some point you will want to knonw what will be committed before you push changes to a git repository. 

This is one of they highly discussed topics on public forums [^5].

[^5]: Read the stackoverflow post [how can i see what i about to push to git](http://stackoverflow.com/questions/3636914/how-can-i-see-what-i-am-about-to-push-with-git) on stackoverflow

Find out the differences between your local copy and the remote repository.
First, you must add the changes to the local branch
```
-> git add
```
Then you can compare with the main branch

I LIKE THIS ONE: compare local copy vs. server
```
-> git diff origin/main 
```
Check the repository activity
```
-> git log 
```
Check what will be committed.
```
-> git diff --staged
-> git diff --cached
```
How can I see what I am about to push with git?
```
-> git diff --stat origin/main HEAD
-> git diff origin/dev1
-> git push origin dev1 --dry-run
```
Use `difftool` on MACOC. Shows a JAVA UI.
```
-> git difftool origin/dev1
```

### Repository Contents - what changed?

Commands executed on the client.
List the current repository contents.
```
-> git ls-files
-> git ls-tree -r main --name-only
-> git ls-tree -r main --full-name
-> git whatchanged
-> git ls-tree --full-tree -r HEAD
```

List what's in a remote repository.
It's not a file listing. It shows the log.
```
-> ssh git@git-servera "cd project1 && git log -n 10"
```

### Display File contents

Display the contents of a file on a branch

Syntax:
```
-> git show <branch>:file
```
Display the contents of a file on dev1 branch and main branch.
```
-> git show dev1:cfile.txt
-> git show main:cfile.txt
```

### LOG

The git log command is used to display a chronological history of commits in a Git repository. It provides details such as commit hashes, author information, dates, and commit messages, allowing users to review the project's development history. The primary purpose of git log is to help users understand the sequence of changes, track modifications over time, and analyze the evolution of the codeb

```
-> git log -p afile.txt	# show change history of a file
-> git log -p -2		# last two commits
-> git log			# show change history of all files

-> git log --oneline | nl -v0 | sed 's/^ \+/&HEAD~/'   # show commits like HEAD~X

-> git log --pretty=oneline
-> git log --pretty=format:"%h %s" --graph
```

## GIT :: Pull

The `git pull` command is for `Merging - Sync` operations.
It ensures you got the latest upstream changes.

{: .important }
It is highly recommended to pull the latest upstream changes before you begin to work on a code base. Failing to do this may bause conflicts that can lead to confusion.

Sync local repository with remote repository
```
-> git pull origin
```
Specify the branch name to sync
```
-> git pull origin <mybranch>
```
The command `git pull` without arguments gives useful information regarding the branches present in the remote repository.
```
$ git pull
```

## GIT Fetch

Force a git pull overriding files on the local repo.
Local changes are lost.
Local commits that have not been pushed will be lost.
```
-> git fetch --all
-> git reset --hard origin/main
```
Then the git reset resets the main branch to what you just fetched. The `--hard` option changes all the files in your working tree to match the files in `origin/main`

This downloads all branches from the repo
```
-> git fetch origin
```
The command git fetch downloads the latest from remote without trying to merge or rebase anything.

## GIT Branches

Branch management in GitHub is a key aspect of collaborative software development, allowing teams to work on different features, fixes, or experiments simultaneously without conflicts. Here are some general concepts:

You can do the following tasks:
- Create, delete or rename a branch
- Create and merge pull requests
- Apply branch protection rules to avoid accidental overrides
- Adopt branch strategy such as long-live or short-live branches

{: .highlight }
It is not possible to work with revision control without a basic understanding of branches.

A good understanding of branch tasks will ensure teams collaborate effectively to maintain code.

### List remote branches

```
-> git branch
-> git branch -a
-> git branch -r
-> git branch -l
```

### Branch Create

Check which branch you're on.
```
-> git branch -v
```
Create new branch (while in the main for example)
```
-> git checkout -b new_branch
```
Push new branch to repository after making changes.
```
-> git add .
-> git commit -am “updates”
-> git push origin new_branch
```

### Pull specific branch

http://stackoverflow.com/questions/1911109/clone-a-specific-git-branch

Pull new branch into your local main branch.
The new_branch on the local repo will be the same name as in the remote repo.
```
-> git pull origin new_branch
-> git checkout <lbranch>
```
Or,
Pull down rbranch name it lbranch locally.
```
-> git fetch <remote> <rbranch>:<lbranch>
-> git checkout <lbranch>
```
Example: pull down branch dev1 from origin
```
-> git fetch origin dev1:dev1test
-> git checkout dev1test
```

### Branch Single Clone

Clone a single branch from a repository.
Need git v1.7.10 to use --single-branch.

I tested this with SSH.
```
-> /usr/local/bin/git clone -b <my_branch> --single-branch git@git-servera:/home/git/project1
```
This is from Stackoverflow.
```
-> git clone -b <my_branch> --single-branch https://github.com/data/pets.git
```

### Branch Delete

Deleting a REMOTE branch (replace origin with whatever name you call it): 
```
-> git push origin --delete <branch> #Git version 1.7.0 or newer 
-> git push origin :<branch>         #Git versions older than 1.7.0 
```
Deleting a LOCAL branch: 
```
-> git branch --delete <branch> 
-> git branch -d <branch>            #Shorter version 
-> git branch -D <branch>            #Force delete unmerged branches 
```
Deleting a local remote-tracking branch:
```
-> git branch -a                     # get a branch listing 
-> git branch --delete --remotes <remote>/<branch> # use the output of the ‘git branch -a
-> git branch -dr <remote>/<branch>  #Shorter 
-> git fetch <remote> --prune        #Delete multiple obsolete tracking branches 
-> git fetch <remote> -p             #Shorter 
```
No need to do git commit or anything after deleting a branch.


### Branch Merge

Sequence to create a new branch, add contents, and merge wit the main.
(!) The files on newBranch will override the files on main.
```
-> git checkout -b newBranch	   # create the branch
-> vi file1				   # edit a file
-> git add file1			   # add the file
-> git commit -m”Edited file1’	   # commit the file
-> git push origin newBranch	   # push the changes to thew new branch
-> git checkout main             # switch to main branch
-> git merge newBranch           # merge the new branch work into main
-> git branch -d newBranch       # delete the newBranch if no longer needed
```

### Default Branch

Set the default branch globally for the current user.
```
-> git config --global init.defaultBranch main
```
All newly created repos will have main as the default branch.
The change is saved in  ~/.gitconfig
```
[init]
  defaultBranch = main
```
In addition to the global setting, change the local repository.
```
-> git branch -m main
```
Check the default branch
```
-> git config --global init.defaultbranch
-> git symbolic-ref --short HEAD
-> grep defaultBranch ~/.gitconfig
```
To change another default branch, in the previous commands just change the branch name to whatever.
Remove the default branch setting. There won’t be one after this.
```
-> git config --global --unset init.defaultBranch
```

## GIT SSH Repositoriy Access

It is possible to access to git repo with SSH Key

Anyone requiring access to the projects on the git server needs to copy their public SSH key to the git user `authorized_keys` file on the git server.
```
[git@git-server]$ cat /home/devuser/.ssh/authorized_keys
```
Test the access to ensure it works.
```
[devuser@git-client] ssh git@git-server echo test
```

## References

[Return to main page]({{site.baseurl}}/).