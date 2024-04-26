---
title: "Reorder commits interactively"
type: doc
subtype: Git
order: 3
---

See [here](https://stackoverflow.com/questions/37471740/how-to-copy-commits-from-one-git-repo-to-another) for more details.

``` bash
# create a license file, edit, add content and save

# add file to git and commit
$ git add LICENSE
$ git commit -m "Initial Commit"

# do an interactive rebase to move commits around
$ git rebase -i --root

# move the last line (last commit) to the top of the file
# the last commit will be then the first one
```
