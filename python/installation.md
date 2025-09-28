# Installation related

## Install projects

Using `uv` and `pip`

### Install current project in editable mode

```shell
$ gso uv pip install -e .
```

<pre>
Resolved 5 packages in 6ms
      Built repoconf @ file:///D:/Company/vaastav-tech-proj/vcs/git/py-repoconf
Prepared 1 package in 1.23s
Uninstalled 1 package in 1ms
░░░░░░░░░░░░░░░░░░░░ [0/1] Installing wheels...                                                                                                                              
warning: Failed to hardlink files; falling back to full copy. This may lead to degraded performance.
         If the cache and target directories are on different filesystems, hardlinking may not be supported.
         If this is intentional, set `export UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
Installed 1 package in 9ms
 ~ repoconf==0.0.0.dev1 (from file:///D:/Company/vaastav-tech-proj/vcs/git/py-repoconf)
<span style="color:red">[GSO]</span> exit code: 0
</pre>

### Uninstall all the projects from uv and pip

```shell
$ uv pip list | tail -n +3 | cut -d" " -f1 | gso xargs uv pip uninstall
```

<pre>
Uninstalled 5 packages in 17ms
 - gitbolt==0.0.0.dev8
 - logician==0.0.0.dev3
 - repoconf==0.0.0.dev1 (from file:///D:/Company/vaastav-tech-proj/vcs/git/py-repoconf)
 - vt-commons==0.0.1
 - vt-err-hndlr==0.0.0.dev1
<span style="color:red">[GSO]</span> exit code: 0
</pre>
