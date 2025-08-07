# 🔒 GPG: Encryption

This section covers how to use GPG for encryption.

## 📌 Modes for GPG encryption

* symmetric: same key can be used for encryption as well as decryption
  * pros:
    * fast.
    * simple.
    * less management.
  * cons:
    * Need a trusted channel to send the symmetric key to the receiver.
    * Many receivers will have the same key for encryption and decryption.
* asymmetric: Different keys for encryption and decryption.
  * pros:
    * no trusted channel required for key transfer.
    * better key and responsibility limitation enforcement.
  * cons:
    * slower than symmetric.
    * complex.
    * more management.

## 📄 Reference Guide

Includes:

* asymmetric encryption with recipient subkeys: [encryption with subkeys](./recipients/encryption-with-subkey.md)
