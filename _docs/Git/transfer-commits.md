---
title: "Transfer commit from one repo to another one"
type: doc
subtype: Git
order: 1
---

``` bash
# add the old repo as a remote repository
$ git remote add oldrepo [<https://github.com>]/path/to/dummy-repo/

# get the old repo commits
$ git remote update

# examine the whole tree
$ git log --all --oneline --graph --decorate

# copy (cherry-pick) the commits from the old repo into your new local one
$ git cherry-pick sha-of-commit-one
$ git cherry-pick sha-of-commit-two
$ git cherry-pick sha-of-commit-three

# check your local repo is correct
$ git log

# send your new tree (repo state) to github
$ git push origin main

# remove the now-unneeded reference to oldrepo
$ git remote remove oldrepo
```