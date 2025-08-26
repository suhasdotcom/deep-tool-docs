# Remote on just init repo

Check what is the remote on a repo that is just initialised

> This project uses [GSO](https://github.com/Vaastav-Technologies/gso).

#### Non bare repo
```shell
$ gso git init remote
```
<pre>
<span style="color:green">Initialized empty Git repository in D:/TestTmpDir/git/remote-on-just-init-repo/non-bare-repo/remote/.git/</span>
<span style="color:red">[GSO]</span> exit code: 0
</pre>

#### Bare repo

```shell
$ gso git init --bare remote
```
<pre>
<span style="color:green">Initialized empty Git repository in D:/TestTmpDir/git/remote-on-just-init-repo/bare-repo/remote/</span>
<span style="color:red">[GSO]</span> exit code: 0
</pre>

### Conclusion

Just initialised repo has no remote set.
