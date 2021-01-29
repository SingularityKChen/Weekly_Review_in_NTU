---
layout: post
title: "[Glean] Branch Strategy"
description: "Version control strategies, like TBD, Git-Flow, GitLab-Flow."
categories: [Glean]
tags: [GitHub, Version Control]
last_updated: 2021-01-29 17:31:00 GMT+8
excerpt: "Version control strategies, like TBD, Git-Flow, GitLab-Flow."
redirect_from:
  - /2021/01/29/
---

* Kramdown table of contents
{:toc .toc}
# Branch Strategy

## TBD

All the developers develop their codes in master.

Each commit should pass the CI.

Each developer should have their own repo.

This method suits small team who wants to develop a single product with single version.

## Git-Flow[^1]

At the core, the development model is greatly inspired by existing models out there. The central repo holds two main branches with an infinite lifetime:

+ `master`
+ `develop`

The different types of branches we may use are:

+ Feature branches
+ Release branches
+ Hotfix branches

This method suits the team who want support one product with multiple versions and has a known release schedule.

However, this can cause a lot merge conflict as it has several branches and each of them can have a long lifetime.

### Master branch

The `master` branch at `origin` should be familiar to every Git user. Parallel to the `master` branch, another branch exists called `develop`.

We consider `origin/master` to be the main branch where the source code of `HEAD` always reflects a *production-ready* state.

### Develop branch

We consider `origin/develop` to be the main branch where the source code of `HEAD` always reflects a state with the latest delivered development changes for the next release. Some would call this the “integration branch”.  This is where any automatic nightly builds are built from.

When the source code in the `develop` branch reaches a stable point and is ready to be released, all of the changes should be merged back into `master` somehow and then tagged with a release number.

### Feature branches

+ May branch off from:

  `develop`  

+ Must merge back into:

  `develop`  

+ Branch naming convention:

  anything except `master`, `develop`, `release-*`, or `hotfix-*`

Feature branches (or sometimes called topic branches) are used to develop new features for the upcoming or a distant future release. 

Feature branches typically exist in developer repos only, not in `origin`.

```bash
$ git checkout -b myfeature develop
Switched to a new branch "myfeature"
```

Finished features may be merged into the `develop` branch to definitely add them to the upcoming release:


```bash
$ git checkout develop
Switched to branch 'develop'
$ git merge --no-ff myfeature
Updating ...
$ git branch -d myfeature
Deleted branch myfeature.
$ git push origin develop
```

The `--no-ff` flag causes the merge to always create a new commit object, even if the merge could be performed with a fast-forward. This avoids losing information about the historical existence of a feature branch and groups together all commits that together added the feature.

### Release branches

+ May branch off from:

  `develop`

+ Must merge back into:

  `develop` and `master`

+ Branch naming convention:

  `release-*`

Release branches support preparation of a new production release. They allow for last-minute dotting of i’s and crossing t’s. Furthermore, they allow for minor bug fixes and preparing meta-data for a release (version number, build dates, etc.).

The key moment to branch off a new release branch from `develop` is when develop (almost) reflects the desired state of the new release. At least all features that are targeted for the release-to-be-built must be merged in to `develop` at this point in time. All features targeted at future releases may not—they must wait until after the release branch is branched off.

First, the release branch is merged into `master` (since every commit on `master` is a new release *by definition*, remember). Next, that commit on `master` must be tagged for easy future reference to this historical version. Finally, the changes made on the release branch need to be merged back into `develop`, so that future releases also contain these bug fixes.

```bash
$ git checkout master
Switched to branch 'master'
$ git merge --no-ff release-1.2
Merge made by recursive.
(Summary of changes)
$ git tag -a 1.2
```

To keep the changes made in the release branch, we need to merge those back into `develop`, though. In Git:

```bash
$ git checkout develop
Switched to branch 'develop'
$ git merge --no-ff release-1.2
Merge made by recursive.
(Summary of changes)
```

Now we are really done and the release branch may be removed, since we don’t need it anymore:

```bash
$ git branch -d release-1.2
```

### Hotfix branches

+ May branch off from:

  `master`

+ Must merge back into:

  `develop` and `master`

+ Branch naming convention:

  `hotfix-*`

Hotfix branches are very much like release branches in that they are also meant to prepare for a new production release, albeit unplanned. They arise from the necessity to act immediately upon an undesired state of a live production version. When a critical bug in a production version must be resolved immediately, a hotfix branch may be branched off from the corresponding tag on the master branch that marks the production version.

[^1]: https://nvie.com/posts/a-successful-git-branching-model/