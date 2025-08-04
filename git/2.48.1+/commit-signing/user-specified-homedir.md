# 🔐 Git Commit Signing with a User-Specified GPG Homedir

It is not always beneficial to keep all your GPG keys in a single (default) homedir. To improve separation of concerns and security, keys can be isolated by their intended use. For example, keys used only for signing commits in a particular repository can live within that repo, such as in a `.git/.gpg-home/` directory.

Placing the keys inside the `.git/.gpg-home` directory ensures that Git does not track them, keeping the keys stored locally and privately.

## 🧪 Example: Signing Commits Using a Custom GPG Home

From your **repository root**, run the following commands:

### 1. 🔧 Generate a GPG key into a local homedir

```bash
mkdir .git/.gpg-home
gpg --homedir=.git/.gpg-home --gen-key
```

### 2. 📝 Register the signing key with Git (locally only!)

* The people who want less control over their git signing keys can simply use a primary key which has an associated signing subkey as such:

    ```bash
    git config --local user.sigingkey <primary-key-with-siging-subkey-from-.git/.gpg-home>
    ```

* The exclamation mark (`!`) appended to the key ID tells Git to use the exact subkey, rather than searching for associated subkeys under a public key.

    ```bash
    git config --local user.signingkey <signing-subkey-id-from-.git/.gpg-home>!
    ```

### 3. ✅ Sign a commit using that key

```bash
GNUPGHOME=.git/.gpg-home git commit -m "some commit message" -S
```

If you haven't configured `user.signingkey`, you can pass the key directly with the `-S` flag:

```bash
GNUPGHOME=.git/.gpg-home git commit -m "some commit message" -S<signing-subkey-id>!
```

> 🔐 The exclamation mark (`!`) is important in both the `user.signingkey` configuration and the `-S` flag during a commit: it tells Git to use that exact subkey, rather than attempting to find a signing-capable subkey associated with a primary key.

> 🧩 For those who prefer less control, using just the primary key (without the `!`) will let Git search for a suitable signing subkey automatically. This can simplify setup, though at the cost of precision and transparency.

Sample Output:

```text
STDOUT: [master (root-commit) 46735a5] added a-file
STDOUT:  1 file changed, 0 insertions(+), 0 deletions(-)
STDOUT:  create mode 100644 a-file

exit code: 0
```

```bash
GNUPGHOME=.git/.gpg-home git commit -m "some commit message" -S
```

Sample Output:

```text
STDOUT: [master (root-commit) 46735a5] added a-file
STDOUT:  1 file changed, 0 insertions(+), 0 deletions(-)
STDOUT:  create mode 100644 a-file

exit code: 0
```

### 4. 🔁 (Optional) Always sign commits in this repo

```bash
git config --local commit.gpgsign true
```

> 💡 Use `--local` to avoid accidentally applying this config to unrelated repositories.

---

This approach provides fine-grained control over GPG key usage, making it easier to maintain security boundaries per project.


### 5. 🤔 What git runs internally when signing commits

```shell
$ GNUPGHOME=.git/.ano-gpg/.gitks/final/ GIT_TRACE=1 git commit -m "adding a-file" -S63DB42567A22A25A!
```

```terminaloutput
12:10:38.134648 exec-cmd.c:266          trace: resolved executable dir: C:/Program Files/Git/mingw64/bin
12:10:38.134648 git.c:476               trace: built-in: git commit -m 'adding a-file' '-S63DB42567A22A25A'\!''
12:10:38.150514 run-command.c:668       trace: run_command: gpg --status-fd=2 -bsau '63DB42567A22A25A'\!''
12:10:38.150514 run-command.c:929       trace: start_command: gpg --status-fd=2 -bsau '63DB42567A22A25A'\!''
error: gpg failed to sign the data:
gpg: keybox '/tmp/pytest-of-suhas/pytest-90/test_centrally_registers_branc0/local/.git/.ano-gpg/.gitks/final/pubring.kbx' created
gpg: skipped "63DB42567A22A25A!": No secret key
[GNUPG:] INV_SGNR 9 63DB42567A22A25A!
[GNUPG:] FAILURE sign 17
gpg: signing failed: No secret key

fatal: failed to write commit object
```

git runs `gpg --status-fd=2 -bsau 'key-id'` to sign commits.


### 6. Change commit signing program

Commit signing and verification program can be changed using the `gpg.program` config key.

```shell
git config --local gpg.program /path/to/your/gpg-or-other-commit-signing/executable
```

[Doc reference](https://git-scm.com/docs/git-config#Documentation/git-config.txt-gpgprogram)
