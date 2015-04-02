# Git training
I'll provide you just a list of useful resources to get started and to understand basic concepts.
In general the best resource for learning git, from the beginning to a very advanced level, is this website (http://git-scm.com/book/en/v2).

It's also an opensource book and you can grab the code [here](http://git-scm.com/book/en/v2)

**I will not show you how to use a GUI like tower or sourcetree. The best and easy way for understanding git is from the command line.**

## Introduction
* [Differences between git and others CVS](http://git-scm.com/book/en/v2/Getting-Started-Git-Basics)
* [Installation](http://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
* [Setting up your environment](https://help.github.com/articles/set-up-git/)
```
git config --global user.name "YOUR NAME"
git config --global user.email "YOUR EMAIL"
```
* Creating/clone a repository
```shell
# from inside a directory
git init
# an existing project
git clone https://github.com/vesparny/git-training.git && cd git-training
```
* Recording changes
```shell
# show differences
git status
# add files
git add .
# committing
git commit -m "my commit message"
```
* unstaging changes
```shell
git reset HEAD <file>...
```
* Unmodifying a modified File
```shell
git checkout -- [file]...
```
It’s important to understand that `git checkout -- [file]` is a dangerous command. Any changes you made to that file are gone – you just copied another file over it.

## Remotes
* [Introduction](http://git-scm.com/book/en/v2/Git-Basics-Working-with-Remotes)
>To be able to collaborate on any Git project, you need to know how to manage your remote repositories. Remote repositories are versions of your project that are hosted on the Internet or network somewhere.
>
```shell
# List them
git remote -v
origin	git@github.com:vesparny/git-training.git (fetch)
origin	git@github.com:vesparny/git-training.git (push)
```

## Branching
* [Introduction](http://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell)
>When you make a commit, Git stores a commit object that contains a pointer to the snapshot of the content you staged. This object also contains the author’s name and email, the message that you typed, and pointers to the commit or commits that directly came before this commit (its parent or parents): zero parents for the initial commit, one parent for a normal commit, and multiple parents for a commit that results from a merge of two or more branches.
Branches are just pointers to a specific commit.

**HEAD file is a pointer to the branch you’re on.**

```shell
# List them
git branches
# Create a new one
git branch testing # or git checkout -b testing
origin	git@github.com:vesparny/git-training.git (fetch)
origin	git@github.com:vesparny/git-training.git (push)
```

## Merging
* [Basic Merging](http://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
* fast-forward merge
>Because the commit pointed to by the branch you merged in was directly upstream of the commit you’re on, Git simply moves the pointer forward.

* recursive merge
>In this case, your development history has diverged from some older point. Because the commit on the branch you’re on isn’t a direct ancestor of the branch you’re merging in, Git has to do some work. In this case, Git does a simple three-way merge, using the two snapshots pointed to by the branch tips and the common ancestor of the two.
>
* merge conflicts
>Happens when you changed the same part of the same file differently in the two branches you’re merging together.

This is the perfect situation for using a mergetool,
I suggest you [kdiff3](http://kdiff3.sourceforge.net/) which is free, open source and multi-platform.

## Rebasing
* [Basic reabasing](http://git-scm.com/book/en/v2/Git-Branching-Rebasing)

### **WARNING**
Rebasing rewrites history. It means that your commits will have a different hash after.
Just use rebasing on feature branches, **never ever** rebase branches that are supposed to be used by someone else (like develop or master).


>The easiest way to integrate the branches, as we’ve already covered, is the `merge` command. However, there is another way. This this is called rebasing. With the `rebase` command, you can take all the changes that were committed on one branch and replay them on another one.

## Worflow with rebase
We are using a git strategy called [Feature branch workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow)
Basically we keep a develop branch always clean, branch off from there a new feature branch and work in it.


create a new branch from develop
`git checkout -b feature`

do a commit
`git touch file.txt`
`git add .`
`git commit -m "add file"`

Now imagine that someone else has pushed on develop branch by a merge or pull request.
You probably need to integrate those changes, so you need to get them, then rebase your feature branch onto develop
`git checkout develop`
`git pull origin develop`
`git checkout feature`
`git rebase develop`

if you have conflicts git will tell you.
`git mergetool`

once solved every conflict tell git that you are done.
`git rebase --continue`

you can abort merge at every moment by doing
`git rebase --abort`

Cool, you have have integrated last changes from `develop` in your `feature` branch, and moved on top of the history your previous commits

**Note that changes your commit hash, so if you need to push you need to force it.**

`git push origin feature -f`

Before doing a merge request is necessary to rebase your work with the `develop` branch.
You'd probably also want to clean your commits you did while working on the feature.

`git reset --soft HEAD~3`

where `3` is the number of commits you want to squash.

This will remove your last 3 commits, and put the changed files in them in your working copy.

You just need to commit them and push your single and clean commit with a good message to the remote feature branch (with -f flag because you have rewritten history here)

You are ready for your merge request, since your branch is just one commit ahead `develop`, the merge will be fast-forward.


## Extras
* [aliases, code completion](http://git-scm.com/book/en/v1/Git-Basics-Tips-and-Tricks)
* [Stashing and cleaning](http://git-scm.com/book/en/v2/Git-Tools-Stashing-and-Cleaning#_git_stashing)
* [Git internals - For nerds](http://git-scm.com/book/en/v1/Git-Internals)
