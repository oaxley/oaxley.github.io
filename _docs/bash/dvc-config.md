---
title: "DVC - Data Version Control"
type: doc
subtype: Bash
order: 2
---

## Configure a remote DVC source (S3)

``` bash
# set the remote S3 for DVC (should contain the bucket_name)
dvc remote add --global s3dvc s3://MY_S3_BUCKET/storage

# set the profile (in .aws/credentials)
dvc remote modify --global s3dvc profile s3dvc

# change the location of the credentials
dvc remote modify --global s3dvc credentialpath ~/.aws/credentials

# change the endpoint URL
dvc remote modify --global s3dvc endpointurl https://hostname.fqdn.com:443

# configure the default remote
dvc config --global core.remote s3dvc

# auto stage the changes from DVC
dvc config --global core.autostage true
```

## Initialize a new DVC repository

``` bash
$ cd my-repository
$ dvc init
```

## Add data to DVC and commit to GIT

``` bash
# add the data to DVC control
# DVC files are automatically staged into Git
$ dvc add data/weights

# commit the files to Git
$ git commit -m "Add model weights to DVC"
```

## Push the data to DVC 

``` bash
$ dvc push
```

---
Website: [dvc - data version control](https://dvc.org/)
