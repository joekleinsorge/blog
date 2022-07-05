---
title: 'Git Cheatsheet'
date: 2021-11-04T08:02:18-04:00
draft: false
toc: true
cover: img/git.webp
tags:
  - git
---

> **_NOTE:_** Apparently I am not as original as I thought, for a more enjoyable read, checkout [ohshitgit](https://ohshitgit.com/)

If you are looking for quality content and a generally useful reference please use these ([this](https://education.github.com/git-cheat-sheet-education.pdf) or [this](https://about.gitlab.com/images/press/git-cheat-sheet.pdf)) wonderful guides instead of this page. This post is really more of a reference for myself on situations I've been in and how to fix them.

## Merging a remote fork from the CLI + VSC

The author of the repo I forked made some cool changes and I want to add them to mine.

```shell
# Checkout your branch
git pull https://github.com/ORIGINAL_OWNER/ORIGINAL_REPO.git BRANCH_NAME
git status
# Accept either their changes or your own
```

## Undo merge on one specific file

Oops, didn't mean to include _that_ file.

```shell
git checkout HEAD~1 -- THE_FILE
```

## Rebasing from remote repo

I messed something up and need to start fresh.

```shell
git reset --hard origin/main
git pull origin main
```

## Updating a remote submodule

The repo in the submodule got updated and I want to pull that in.

```shell
git submodule update --remote --merge
```

## Make a new branch with current changes

Because I forgot to make one before I started editing and direct commits to Main are discouraged.

Only works if I haven't committed anything yet.

```shell
git checkout -b NEW_BRANCH
```

## Push new branch to remote repo

Because I created a new one for this post, but GitHub doesn't know about it yet.

```shell
git push --set-upstream origin git-post
```
