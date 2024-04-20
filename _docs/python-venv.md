---
title: "Python Virtual Environment"
type: doc
order: 1
---

### Create a new virtual environment

This will create a new Python virtual environment in the `venv` sub-directory.  
The prompt for this environment will be `DUDe`.

``` bash
$ python -m venv --prompt DUDe venv
```

### Activate a virtual environment

``` bash
$ source venv/bin/activate
(DUDe) $
```

### Deactivate the virtual environment

``` bash
(DUDe) $ deactivate
$
```
