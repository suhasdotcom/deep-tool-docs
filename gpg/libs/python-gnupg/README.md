# Python GnuPG project related observations

## Pycharm on windows

A peculiar observation presented itself today that this code snippet:

```python
import gnupg

gpg = gnupg.GPG()
gpg.list_keys()
```

Produced a set of keys when the python process (be it console, REPL, terminal or tests) ran in PyCharm and a
totally different set of keys when ran outside PyCharm. Usually the gpg `homedir` is the gpg filesystem path on which
keys to consider. If no `homedir` is supplied as a kwarg to `gnupg.GPG()` then
it defaults to whatever is the default home-dir for gnupg. By default, gnupg uses `~/.gnupg` as its default home-dir.

It was observed when operating via PyCharm on Windows, the gnupg (be it python gnupg project or run directly from
`subprocess`)
considered `~/AppData/Roaming/gnupg` as its default. Maybe PyCharm sets the `GNUPGHOME` env-var to sandbox gnupg
operations when done inside PyCharm. This is a little confusing and hence, is being included in this documentation
project.

> Tip: Better to set a consistent `homedir` to avoid surprises later.
