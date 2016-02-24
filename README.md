# Git Workflow

Branching and Release Management Strategy Guide

[Git Commands Reference](./commands.md)

----


**Purpose of this Document**

The purpose of this document is to serve as an **official** reference guide for an efficient VCS branching and deployment model using git.

The strategy described in this document also serves as a foundation for comparison when proposing changes to improve or alter the process per requirements.

This document can be managed within a version control system in order for historical changes to be tracked.

**References**

- [A Successful Git Branching Model](http://nvie.com/posts/a-successful-git-branching-model/)
- [Atlassian Tutorial - Gitflow Workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)
- [Why aren't you using Git Flow?](http://jeffkreeftmeijer.com/2010/why-arent-you-using-git-flow/)
- [Git Documentation](http://git-scm.com/documentation)



## Introduction to Git Flow

This Git Workflow is derived from Vincent Driessen's blog post from 2010: [A Successful Git Branching Model](http://nvie.com/posts/a-successful-git-branching-model/).

This model works because there are a strict set of procedures that must be followed by every team member for the development process.

**Goals of this model:**

- Keep repository history organized
- Simplify reference to specific release versions and hotfixes
- Implement a solid and scalable branching model that can be applied to all projects
- Flexible for developers, strict for quality deployments
- Flexible code management for staging and testing

#### Branching Diagram
![Branching Diagram](http://nvie.com/img/git-model@2x.png)



----

## Branches

Git Flow involves five different branches

| Branch           | Naming Convention          |
| :---             | :---                       |
| Master           | `master`                   |
| Development      | `develop`                  |
| Feature Branches | `feature/*`                |
| Release Branches | `release/*` or `release-*` |
| Hotfix Branches  | `hotfix/*` or `hotfix-*`   |



### Main Branches

The main branches are permanent for the lifetime of the project.

**Master Branch**

The master branch is used only for stable releases and contains production-ready code. Every commit to the master branch is tagged immediately.
Commits are never made directly but are merged in from hotfix or release branches.

**Develop Branch**

This branch serves as the integration branch for new features and is used for the bulk of daily development.
The develop branch is where the more general, shared development and bug-fixes commits can be directly made, as well as where the feature, release, or hotfix branches are merged to.


### Supporting Branches


**Feature Branches**

Feature branches are used to develop new features for the next upcoming or scheduled distant future release.  When tagged with a future release version within a project management system, developers and clients have a common understanding of what is expected to be done for a release.

A feature branch should only exist while the feature is in development and it will ultimately be merged into `develop` or removed if it was just for experimenting.`

New feature branches are created off of the `develop` branch.  Once the feature branch has been merged into `develop`, further updates to the code can be commited directly in the `develop` branch.


**Release Branches**

Release branches support preparation of a new production release.  A `release` branch can allow for proper quality testing and minor bug-fixing without delaying development as `develop` is prepared for the next release.


**Hotfix Branches**

`Hotfix` branches are very much like release branches in that they are also meant to prepare for a new production release, albeit unplanned. They arise from the necessity to act immediately upon an undesired state of a live production version. When a critical bug in a production version must be resolved immediately, a hotfix branch may be branched off from the corresponding tag on the master branch that marks the production version.

[Hotfix Branching Example](http://nvie.com/img/hotfix-branches@2x.png)

### Branching Summary



**Main Branches**

| Branch      | Type | Naming Convention | Lifetime  |
| :---        | :--- | :---:             | :---:     |
| Master      | Main | `master`          | Permanent |
| Development | Main | `develop`         | Permanent |


**Supporting Branches**

| Branch           | Type       | Naming Convention | Lifetime  | Branched From | Merged To              |
| :---             | :---       | :---:             | :---:     | :---:         | :---:                  |
| Feature Branches | Supporting | `feature/*`       | Temporary | `develop`     | `develop`              |
| Release Branches | Supporting | `release/*` or `release-*`       | Temporary | `develop`     | `master` and `develop`  |
| Hotfix Branches  | Supporting | `hotfix/*` `hotfix-*`        | Temporary | `master`      | `master` and `release` |



## Version Tagging

[Semantic Versioning](http://semver.org/) should be used for tagging releases, and hotfixes.

### Summary
Given a version number MAJOR.MINOR.PATCH, increment the:

* MAJOR version when you make incompatible API changes
* MINOR version when you add functionality in a backwards-compatible manner
* PATCH version when you make backwards-compatible bug fixes.

### Pre-release Note

Additional labels for pre-release and build metadata are available as extensions to the MAJOR.MINOR.PATCH format.

**Examples:** 1.0.0-alpha, 1.0.0-alpha.1, 1.0.0-0.3.7, 1.0.0-x.7.z.92.


****

# Git Flow Usage Example


## Developing a Feature Branch

**Goals:**

1. Create a new feature branch based off the latest code from the `develop` branch.
2. Write code and apply commits to the new feature branch
3. Merge finished feature branch into `develop` branch

----

**Step 1:** Create Feature Branch from `develop`

Make sure you are on the `develop` branch and it is up-to-date with remote.

`$ git checkout develop`

>```
Switched to branch 'develop'
Your branch is behind 'origin/develop' by 1 commit, and can be fast-forwarded.
(use "git pull" to update your local branch)
```


**Step 2:** Pull Latest Changes from remote

`$ git pull`

>```
Updating 0e55abe..6a56d71
Fast-forward
develop.txt | 1 +
1 file changed, 1 insertion(+)
create mode 100644 develop.txt
```

**Step 3:** Create Feature Branch

`$ git checkout -b feature/XX-5 develop`

>```
Switched to a new branch 'feature/XX-5'
```

**Step 4:** Add New Files and Commit with Messages

```
$ git add .\gitflow_guidelines.md
$ git commit -m "XX-5 - Example guidelines file"
```

>```
[feature/XX-5 c4a2d5c] XX-5 - Example guidelines file
 1 file changed, 5 insertions(+)
 create mode 100644 gitflow_guidelines.md
```

**Step 5:** Checkout develop branch and merge with --no-ff

`$ git checkout develop`

>```
Switched to branch 'develop'
Your branch is up-to-date with 'origin/develop'.
```

`$ git merge --no-ff feature/XX-5`

>```
Updating c316181..c4a2d5c
Fast-forward
 gitflow_guidelines.md | 5 +++++
 1 file changed, 5 insertions(+)
 create mode 100644 gitflow_guidelines.md
```

**Step 6:** Remove feature branch

`$ git branch -d feature/XX-5`

>`Deleted branch feature/XX-5 (was c4a2d5c).`


**Step 7:** Push updated develop branch to remote

`$ git push origin develop`

>```
Counting objects: 3, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 343 bytes | 0 bytes/s, done.
Total 3 (delta 1), reused 0 (delta 0)
To https://github.com/df2k2/std.git
   c316181..c4a2d5c  develop -> develop
```



## Create Release Branch, Set Version Tag, and Merge for Deployment

**Goals:**

- Create new `release` branch off `develop` and include version in branch name
- Update version manually or with a script and commit with message for new version
- Checkout `master`, merge `release-1.0.0`, add annotated `git tag`
- Checkout `develop`, merge `release-1.0.0` into `develop` branch
- Remove release branch


**Step 1:** Create new `release` branch, apply version update, and commit with message.

`$ git checkout -b release-1.0.0 develop`

>`Switched to a new branch 'release-1.0.0'`


`$ ./bump-version.sh 1.0.0`

>`Updated version to 1.0.0`

_Note: Here, bump-version.sh is a fictional shell script that changes some files in the working copy to reflect the new version. (This can of course be a manual change—the point being that some files change.) Then, the bumped version number is committed._


`$ git commit -a -m "Bumped to version 1.0.0"`

>```
[release-1.0.0 44293cf] Bumped to version 1.0.0
 1 files changed, 1 insertions(+), 1 deletions(-)
```


**Step 2:** Checkout `master` branch and merge the release branch with the --no-ff switch

`$ git checkout master`

>`Switched to branch 'master'`

`$ git merge --no-ff release-1.0.0`

>```
Merge made by recursive.
 VERSION          |    1 +
 sample/README.md |    7 +++++++
 2 files changed, 8 insertions(+), 0 deletions(-)
 create mode 100644 VERSION
 create mode 100644 sample/README.md
```


**Step 3:** Add version tag with `git tag -a`

`$ git tag -a 1.0.0`

**Step 4:** Push to remote master for deployment

`$ git push origin master`

>```
Counting objects: 9, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (6/6), done.
Writing objects: 100% (7/7), 11.82 KiB, done.
Total 7 (delta 3), reused 0 (delta 0)
To https://df2k2@github.com/df2k2/guides
   6021ec6..2a61e75  master -> master
```

**Step 5:** Repeat same steps for merging except with the `develop` branch

`$ git checkout develop`

>`Switched to branch 'develop'`

`$ git merge --no-ff release-1.0.0`

>```
Merge made by recursive.
 VERSION |    1 +
 1 files changed, 1 insertions(+), 0 deletions(-)
 create mode 100644 VERSION
```

**Step 6:** Delete release-1.0.0 branch

`$ git branch -d release-1.0.0`

>`Deleted branch release-1.0.0 (was 44293cf).`


**Step 7:** Push updates to remote

`$ git push origin develop`

>```
Counting objects: 1, done.
Writing objects: 100% (1/1), 234 bytes, done.
Total 1 (delta 0), reused 0 (delta 0)
To https://df2k2@github.com/df2k2/guides
   ab54a4d..d4fcf79  develop -> develop
```

**Optionally Verify Tag and Show Tag Commit History**

`$ git tag`

>`1.0.0`

View History for 1.0.0

`$ git show 1.0.0`

>```
tag 1.0.0
Tagger: Chris S <df2002@gmail.com>
Date:   Wed Feb 24 08:02:43 2016 -0500
Updated to 1.0.0
commit 2a61e754b91f293677de297d7f1a929b483172c2
Merge: 6021ec6 44293cf
Author: Chris S <df2002@gmail.com>
Date:   Wed Feb 24 08:02:38 2016 -0500
Merge branch 'release-1.0.0'
```


**Step 8:** Push tag to remote

Push all tags with:

`$ git push origin --tags`

Push single tag with:

`$ git push origin 1.0.0`

>```
Counting objects: 1, done.
Writing objects: 100% (1/1), 161 bytes, done.
Total 1 (delta 0), reused 0 (delta 0)
To https://df2k2@github.com/df2k2/guides
 * [new tag]         1.0.0 -> 1.0.0
```



## Managing a Hotfix
