

# Contribute

Welcome to the Contributors Guide. This documentation aims to document how contributors and collaborators should work when using Git, GitHub and the development workflow. This workflow supports the parallel development on multiple major versions. However, older versions only get bug fixes and no new features. This Git workflow is **greatly** inspired by the development workflow of the [Doctrine Projects](http://www.doctrine-project.org/) (in fact, it is same one, with some modifications related to the this project).

# About
Naut (Not Another Useless Todolist) is a multiplatform software to help you and your organization to deal with individual productivity managing tasks.

# Contributors vs Collaborators
Before continuing you need to understand the difference between a contributor and a collaborator.

* Contributor: A contributor is someone from the outside not on the core development team of the project that wants to contribute some changes to a project.
* Collaborator: A collaborator is someone on the core development team of the project and has commit access to the main repository of the project.

Continue reading to learn about the workflow for both contributors and collaborators.

# Contributor Workflow

## Overview

![Git Development Workflow](https://www.grafikart.fr/uploads/2014/02/git-workflow-release-cycle-feature.png)

## Who is a contributor? 
A contributor can be anyone! It could be you. Continue reading this section if you wish to get involved and contribute back to naut.

## Initial Setup
* Setup a [GitHub](https://github.com/) account
* Fork the [repository](https://github.com/tealpaladin/naut) of the project
* Clone your fork locally


```bash
$ git clone https://github.com/username/naut.git
```

* Enter the naut directory and add the upstream remote

```bash
$ cd naut
$ git remote add upstream https://github.com/tealpaladin/naut.git
```

## Keeping your master up-to-date!
Once all this is done, you’ll be able to keep your local master up to date with the simple command:

```bash
$ git checkout master
$ git pull --rebase
```

## Branching Model
The following names will be used to differentiate between the different repositories:

* **upstream** - The “official” naut repository (what is on Structured Dynamics's GitHub account)
* **origin** - Your fork of the official repository on GitHub (what is on your GitHub account)
* **local** - This will be your local clone of **origin** (what is on your computer)

As a **contributor** you will push your completed **local** topic branch to **origin**. As a **contributor** you will pull updates from **upstream**. As a **collaborator** (write-access) you will merge branches from **contributors** into **upstream**.

## Primary Branches
The upstream repository holds the following primary branches:

* **upstream/master** Development towards the next release.
* **upstream/x.y.z** Maintenance branches of existing releases.

These branches exist in parallel, are permanent and are defined as follows:

**upstream/master** is the branch where the source code of **HEAD** always reflects the latest version. Each released major stable version will be a tagged commit in a **upstream/x.y.z** branch.

> **NOTE** You should never commit to your forked **origin/master**. Changes to **origin/master** will never be merged into **upstream/master**. All work must be done in a **topic branch**, which are explained below.

## Topic Branches
Topic branches are for contributors to develop bug fixes, new features, etc. so that they can be easily merged to **master**. They must follow a few simple rules as listed below:

* May branch off from: **master** or a **x.y.z** branch.
* Must merge back into: **master** and any affected **x.y.z** branch that should get the same changes, but remember that release branches usually only get bug fixes, with rare exceptions.
* Branch naming convention: anything except **master** and **x.y.z**. If the topic is related to a [GitHub issue on the naut (**upstream**) project](https://github.com/tealpaladin/naut/issues?direction=desc&sort=comments&state=closed), then name it **topic-#** where **#** is the number of the GitHub issue, i.e. **topic-13**. You should consider creating an issue on that issue tracker **before** starting a new topic branch. That way, people will be able to know what you are doing with your topic branch.

Topic branches are used to develop new features and fix reported issues. When starting development of a feature, the target release in which this feature will be incorporated may well be unknown. The essence of a topic branch is that it exists as long as the feature is in development, but will eventually be merged back into **master** or a **x.y.z** branch (to add the new feature or bugfix to a next release) or discarded (in case of a disappointing experiment).

Topic branches should exist in your **local** and **origin** repositories only, there is no need for them to exist in **upstream**.

## Working on topic branches
First create an appropriately named branch. When starting work on a new topic, branch off from **upstream/master** or a **upstream/x.y.z** branch:

```bash
$ git checkout -b topic-13 upstream/master
Switched to a new branch "topic-13"
```
Now do some work, make some changes then commit them:

```bash
$ git status
$ git commit <filespec>
```

Next, merge or rebase your commit against **upstream/master**. With your work done in a **local** topic branch, you’ll want to assist upstream merge by rebasing your commits. You can either do this manually with `fetch` then `rebase`, or use the `pull --rebase` shortcut. You may encounter merge conflicts, which you should fix and then mark as fixed with add, and then continue rebasing with `rebase --continue`. At any stage, you can abort the `rebase` with `rebase --abort` unlike nasty merges which will leave files strewn everywhere.

> CAUTION Please note that once you have pushed your branch remotely you MUST NOT rebase!

```bash
$ git fetch upstream
$ git rebase upstream/master topic-13
```

or (uses tracking branch shortcuts):

```bash
$ git pull --rebase
```
> **CAUTION** You must not rebase if you have pushed your branch to **origin**.

If you need to pull master into your branch after it has already been pushed remotely, simply use:

```bash
$ git pull
```
Push your branch to **origin**:

Finished topic branches should be pushed to **origin** for a **collaborator** to review and pull into **upstream** as appropriate:

```bash
$ git push origin topic-13
To git@github.com:hobouser/naut.git
    * [new branch]      topic-13 -> topic-13
```

Now you are ready to send a pull request from this branch, and update the GitHub issue tracker, to let a collaborator know your branch can be merged.

## Topic Branch Cleanup
Once your work has been merged by the branch maintainer, it will no longer be necessary to keep the local branch or remote branch, so you can remove them!

Sync your local master:

```bash
$ git checkout master
$ git pull --rebase
```

Remove your local branch using `-d` to ensure that it has been merged by upstream. Branch `-d` will not delete a branch that is not an ancestor of your current head.

From the git-branch man page:

```bash
-d  Delete a branch. The branch must be fully merged in HEAD.
-D  Delete a branch irrespective of its merged status.
```

Remove your local branch:

```bash
$ git branch -d topic-13
```

Remove your remote branch at origin:

```bash
$ git push origin :topic-13
```

# Collaborator Workflow
## Who is a collaborator?

Collaborators are those who have been granted write access to the main repository of a project. This repository will be referred to as **upstream** in this document.

You might want want to know how a collaborator is different from a contributor. The **Collaborator Workflow** is used primarily for the following:

* Merging **contributor** branches into **upstream/master** and/or **upstream/x.y.z** branches.
* Creating **x.y.z** branches.
* Tagging released versions within **master** and **x.y.z** branches.

## Branching Model
### Merging topic branches
* Topic branches **must** merge into **master** and/or any affected **x.y.z** branches.
* Merging a topic branch puts it into the _next_ release, that is the next release created from **master** and/or the next patch release created from a specific **x.y.z** branch.

### Steps
Add remote repo for contributor/collaborator, if necessary (only needs to be done once per collaborator):

```bash
$ git remote add bobuser https://github.com/bobuser/naut.git
```

`Fetch` remote:

```bash
$ git fetch bobuser
```

Merge **topic branch** into **master**:

```bash
$ git checkout master
Switched to branch 'master'
$ git merge --no-ff bobuser/topic-13
Updating ea1b82a..05e9557
(Summary of changes)
$ git push upstream master
```

The `–no-ff` flag causes the merge to always create a new commit object, even if the merge could be performed with a fast-forward. This avoids losing information about the historical existence of a topic branch and groups together all commits that together added the topic.

## Release branches
* May branch off from: **master**
* Must merge back into: **-**
* Branch naming convention: **x.y.z**

Release branches are created when **master** has reached the state of the next major or minor release. They allow for continuous bug fixes and patch releases of that particular release until the release is no longer supported.

The key moment to branch off a new release branch from **master** is when **master** reflects the desired state of the new release.

## Creating a release branch
Release branches are created from the **master** branch. When the state of **master** is ready for the upcoming target version we branch off and give the release branch a name reflecting the target version number. In addition the ”.0” release is tagged on the new release branch:

```bash
$ git checkout -b 1.1 upstream/master
Switched to a new branch "1.1"
$ git push upstream 1.1
$ git tag -a 1.1.0
$ git push upstream 1.1
```

This new branch may exist for a while, at least until the release is no longer supported. During that time, bug fixes are applied in this branch (in addition to the **master** branch), if it is affected by the same bug. Adding large new features here is prohibited. They must be merged into **master**, and therefore, wait for the next major or minor release.
