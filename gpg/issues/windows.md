# GPG issues on windows

## First startup issue

### Observation

It may appear as gpg is unresponsive on windows when trying to decrypt something or list keys. Most probable causes are:

- Existence of wrong sized lock files (mostly 0KB)
  - gnupg_spawn_agent_sentinel.lock
  - gnupg_spawn_dirmgr_sentinel.lock
  - gnupg_spawn_keyboxd_sentinel.lock
  - pubring..kbx.lock
- gpg-agent not running and take a lot of time to startup.

### Possible solutions

- Not really sure what the sentinel and lock files do but deleting them certainly helped. 
- Start the gpg-agent on startup if possible or using these commands
  ```shell
  gpg-connect-agent killagent /bye
  ```
  ```terminaloutput
  OK closing connection
  ```
  ```shell
  gpg-connect-agent /bye # delete the sentinel files if this command errs
  ```
  ```terminaloutput
  gpg-connect-agent: no running gpg-agent - starting '/usr/bin/gpg-agent'
  gpg-connect-agent: waiting for the agent to come up ... (5s)
  gpg-connect-agent: connection to the agent established
  ```
- Configure gpg-agent or equivalent in GPG4Win to start at computer startup.


## Different keys on gpg on cmd and gpg on bash

```shell
gpg --list-keys
```

Produces different set of keys when run on any Windows terminal and a totally different set of keys when run from 
git bash.

Further investigation revealed that gpg uses homedir as:
- `C:\Users\<user>\AppData\Roaming\gnupg` - when run from Windows terminal or anything other than git bash
- `C:\Users\<user>\.gnupg` - when run from git bash on Windows.

This may even stem from the installation of GPG4Win along with gpg that comes bundled in git.

> Tip: Better to set a consistent `homedir` to avoid surprises later.
