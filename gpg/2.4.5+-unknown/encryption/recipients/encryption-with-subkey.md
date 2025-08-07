# Encrypt with subkeys

* Encryption can be directly done using the primary key as such:
    ```shell
    gpg --encrypt --armor file-to-encrypt --recipient 8EAA278178F0756549458FB5814378F974576A86
    ```
  This produces a file named file-to-encrypt.asc (ascii or armoured encrypted text). `--armor` is useful if the encrypted file is to be sent via email or something supporting ASCII charset. `--recipient` encrypts the file only for the said primary key owner.
* Better to encrypt to a subkey if subkey is known, using th subkey! syntax:
    ```shell
    gpg --encrypt --armor file-to-encrypt --recipient 8616035453BC2A73!
    ```
  This too produces a file names file-to-encrypt.asc but now this file is encrypted only for the said subkey owner, i.e., **if this subkey was removed from the primary key then the file will not be decryptable**.
