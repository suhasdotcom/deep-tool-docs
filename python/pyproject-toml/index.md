# pyproject.toml related help

### Specify a dependency

```toml
dependencies = [
    "...", ...
]
```


##### Specifying git dependencies

###### Local

On windows:

```toml
dependencies = [
    "lib-proj @ git+file:///d:/TestTmpDir/python/project-dependency-check/optional-dep-check/lib-proj" # by default goes to main or master branch.
]
```

###### Remote

```toml
dependencies = [
    "lib-proj @ git+https://github.com/user/repo.git", # by default goes to main or master branch.
    "sub-lib-pkg @ git+https://github.com/user/repo.git#subdirectory=path/to/package", # by default goes to main or master branch.
    "tag-dep @ git+https://github.com/user/repo.git@v0.0.1",
    "branch-dep @ git+https://github.com/user/repo.git@some-branch",
    "commit-dep @ git+https://github.com/user/repo.git@0aef345c", # not really recommended, better to tag it.
]
```

> Note: dependencies on git for local can be specified similar to remote, i.e. @, # etc symbols work locally too.


##### Specifying local dependencies

On windows:

```toml
dependencies = [
    "lib-proj @ file:///D:/TestTmpDir/python/project-dependency-check/optional-dep-check/lib-proj"
]
```

Note the:
- Three slashes after `file:`.
- Colon after drive letter.
- Drive letter is case-insensitive.
- All paths POSIX style, i.e. forward slashes.


##### Specifying optional-dependencies on client side

```toml
dependencies = [
    "lib-proj[cli] @ file:///d:/TestTmpDir/python/project-dependency-check/optional-dep-check/lib-proj"
]
```

Note the:
- `[cli]` where `cli` is declared as an optional dependency in lib-proj as such:
    ```toml
    [project.optional-dependencies]
    cli =["click"]
    ```

> Note: the optional dependency is not required to be installed in the lib-proj if client-proj wants to use it.
