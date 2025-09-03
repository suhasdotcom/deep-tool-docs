# `git config` related tests

## storing and retrieving configurations from a blob

To get configurations stored in `repoconf/main` branch's `HEAD`'s `repoconfig` file.

```shell
# $ git config --blob repoconf/main:.repoconfig --get keyserver.url
# Note that the config file name must not start with a dot (.), hence .repoconfig is not acceptable
$ git config --blob repoconf/main:repoconfig --get keyserver.url
```

> Note: that the config file name must not start with a dot (.), hence .repoconfig is not acceptable.
