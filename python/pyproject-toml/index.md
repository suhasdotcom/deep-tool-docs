# pyproject.toml related help

### Specify a dependency

```toml
dependencies = [
    "...", ...
]
```

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
