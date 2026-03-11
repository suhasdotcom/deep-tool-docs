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
