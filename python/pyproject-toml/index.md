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
