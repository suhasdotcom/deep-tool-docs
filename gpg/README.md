# 🔐 GPG Utilities and Notes

This directory contains guides, tips, and configuration references related to GnuPG (GPG) usage within development workflows.

## 📁 Contents

* [Keys nuances](./2.4.5+-unknown/keys.md)
* [Keyring exploration](./2.4.5+-unknown/keyrings.md)
* `user-specified-homedir.md` — Guide on using a custom GPG home directory for scoped keys.
* `user-specified-keyring.md` — Guide on using a custom GPG keyring for scoped keys.

## 📌 Scope

This section is useful for developers who:

* Want to isolate GPG keys by usage (e.g., one set for Git commits, another for email encryption).
* Prefer not to pollute the global GPG configuration or keyring.
* Need precise control over which subkey is used for signing commits.

## 📦 Future Additions

Contributions are welcome!

Additional documents may include:

* Best practices for GPG key management.
* Automating GPG configuration in CI/CD environments.
* Troubleshooting signature verification issues.

Feel free to open a PR if you have tips, workflows, or corrections to share.

---

📎 For more general Git signing configuration, see related entries in the `git/commit-signing` section.

```
> This is an evolving repo — built to make working with gpg a little less mysterious and a lot more powerful.
```

### Learning resources

- [Subkeys and why to use them?](https://wiki.debian.org/Subkeys?action=show&redirect=subkeys)
