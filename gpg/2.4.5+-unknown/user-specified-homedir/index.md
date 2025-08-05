# 🗂️ GPG: Using a User-Specified Homedir

This section covers how to use a custom GPG home directory—either via the `--homedir` flag or by setting the `GNUPGHOME` environment variable—for better isolation and control over key usage.

## 📌 Why Use a Custom Homedir?

* To avoid cluttering your default keys.
* To maintain project-specific keys.
* To enforce tighter scoping of trust and configuration.

## Setting homedir

Can be set by:

* using `--homedir` switch, as such:
    ```shell
    gpg --homedir=path/to/my/homedir ...options
    ```
* setting `GNUPGHOME` env var, as such:
    ```shell
    GNUPGHOME=path/to/my/homedir gpg ...options
    ```


## 📄 Reference Guide

Includes:

* Creating GPG keys in a \`user/specified/home/directory\` directory [generate-keys](./generate-keys.md).
* Tips on exact key usage (with `!` for subkey precision).
* How to isolate different keys on homedirs.
* Exporting keys, subkeys and secrets [export-keys](./export-keys.md).

## ✅ Suitable For

* Developers managing multiple identities.
* Teams with internal signing infrastructure.
* CI/CD setups needing environment-scoped GPG configs.
