---
title: "Reset Commits w/o push"
type: doc
subtype: Git
order: 3
---

### Reset the last commit (not pushed yet)

``` bash
$ git reset HEAD~1
```

### Reset the last commit and push to remove

``` bash
$ git reset --hard HEAD~1
$ git push --force origin develop
```
