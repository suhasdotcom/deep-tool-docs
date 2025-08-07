# Encrypt with subkeys

[Debian subkey explanation](https://wiki.debian.org/Subkeys?action=show&redirect=subkeys)

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


## Workflows

Using the `--homedir`, `--no-default-keyring` and `--keyring` switches to isolate this key from the global keys.

More information about this in [user specified homedir](../../user-specified-homedir/index.md)

### File encrypted for a specific subkey of a recipient

#### Generate a primary key

```shell
$ gpg --homedir=base-keys/ --no-default-keyring --keyring=base-keys/base-keyring --expert --full-gen-key
```

```terminaloutput
gpg (GnuPG) 2.4.5-unknown; Copyright (C) 2024 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Please select what kind of key you want:
   (1) RSA and RSA
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
   (7) DSA (set your own capabilities)
   (8) RSA (set your own capabilities)
   (9) ECC (sign and encrypt) *default*
  (10) ECC (sign only)
  (11) ECC (set your own capabilities)
  (13) Existing key
  (14) Existing key from card
Your selection? 11

Possible actions for this ECC key: Sign Certify Authenticate
Current allowed actions: Sign Certify

   (S) Toggle the sign capability
   (A) Toggle the authenticate capability
   (Q) Finished

Your selection? S

Possible actions for this ECC key: Sign Certify Authenticate
Current allowed actions: Certify

   (S) Toggle the sign capability
   (A) Toggle the authenticate capability
   (Q) Finished

Your selection?
Please select which elliptic curve you want:
   (1) Curve 25519 *default*
   (2) Curve 448
   (3) NIST P-256
   (4) NIST P-384
   (5) NIST P-521
   (6) Brainpool P-256
   (7) Brainpool P-384
   (8) Brainpool P-512
   (9) secp256k1
Your selection?
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 3m
Key expires at Mon Nov  3 18:00:43 2025 IST
Is this correct? (y/N) y

GnuPG needs to construct a user ID to identify your key.

Real name: ss-1
Email address: ss-1@ss.ss
Comment:
You selected this USER-ID:
    "ss-1 <ss-1@ss.ss>"

Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? o
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
gpg: revocation certificate stored as '/d/TestTmpDir/gpg/gpg-subkey-secrets-exports/base-keys/openpgp-revocs.d/8EAA278178F0756549458FB5814378F974576A86.rev'
public and secret key created and signed.

pub   ed25519 2025-08-05 [C] [expires: 2025-11-03]
      8EAA278178F0756549458FB5814378F974576A86
uid                      ss-1 <ss-1@ss.ss>

```

#### Generate an encryption subkey

```shell
$ gpg --homedir=base-keys/ --no-default-keyring --keyring=base-keys/base-keyring --edit-key 8EAA278178F0756549458FB5814378F974576A86
```

```terminaloutput
gpg (GnuPG) 2.4.5-unknown; Copyright (C) 2024 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Secret key is available.

sec  ed25519/814378F974576A86
     created: 2025-08-05  expires: 2025-11-03  usage: C
     trust: ultimate      validity: ultimate
[ultimate] (1). ss-1 <ss-1@ss.ss>

gpg> list

sec  ed25519/814378F974576A86
     created: 2025-08-05  expires: 2025-11-03  usage: C
     trust: ultimate      validity: ultimate
[ultimate] (1). ss-1 <ss-1@ss.ss>

gpg>

sec  ed25519/814378F974576A86
     created: 2025-08-05  expires: 2025-11-03  usage: C
     trust: ultimate      validity: ultimate
[ultimate] (1). ss-1 <ss-1@ss.ss>

gpg> addkey
Please select what kind of key you want:
   (3) DSA (sign only)
   (4) RSA (sign only)
   (5) Elgamal (encrypt only)
   (6) RSA (encrypt only)
  (10) ECC (sign only)
  (12) ECC (encrypt only)
  (14) Existing key from card
Your selection? 12
Please select which elliptic curve you want:
   (1) Curve 25519 *default*
   (4) NIST P-384
   (6) Brainpool P-256
Your selection?
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 20250806T215000
Key expires at Thu Aug  7 03:20:00 2025 IST
Is this correct? (y/N) N
Key is valid for? (0) 20250806T162000
Key expires at Wed Aug  6 21:50:00 2025 IST
Is this correct? (y/N) y
Really create? (y/N) y
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.

sec  ed25519/814378F974576A86
     created: 2025-08-05  expires: 2025-11-03  usage: C
     trust: ultimate      validity: ultimate
ssb  cv25519/8616035453BC2A73
     created: 2025-08-06  expires: 2025-08-06  usage: E
[ultimate] (1). ss-1 <ss-1@ss.ss>

gpg>

sec  ed25519/814378F974576A86
     created: 2025-08-05  expires: 2025-11-03  usage: C
     trust: ultimate      validity: ultimate
ssb  cv25519/8616035453BC2A73
     created: 2025-08-06  expires: 2025-08-06  usage: E
[ultimate] (1). ss-1 <ss-1@ss.ss>

gpg>

sec  ed25519/814378F974576A86
     created: 2025-08-05  expires: 2025-11-03  usage: C
     trust: ultimate      validity: ultimate
ssb  cv25519/8616035453BC2A73
     created: 2025-08-06  expires: 2025-08-06  usage: E
[ultimate] (1). ss-1 <ss-1@ss.ss>

gpg> save
```

Now `8616035453BC2A73` is the encryption subkey of `8EAA278178F0756549458FB5814378F974576A86` primary key, which has a secret key `814378F974576A86`.

#### Encrypt a file for this subkey as recipient

```shell
$ gpg --homedir=base-keys/ --no-default-keyring --keyring=base-keys/base-keyring --recipient 8616035453BC2A73! --encrypt --armor file-to-encrypt
```

This created an `file-to-encrypt.asc` file

#### Delete the `8616035453BC2A73` subkey and add another subkey just to check decryption

```shell
$ gpg --homedir=base-keys/ --no-default-keyring --keyring=base-keys/base-keyring --expert --edit-key 8EAA278178F0756549458FB5814378F974576A86
```

```terminaloutput
gpg (GnuPG) 2.4.5-unknown; Copyright (C) 2024 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Secret key is available.

sec  ed25519/814378F974576A86
     created: 2025-08-05  expires: 2025-11-03  usage: C
     trust: ultimate      validity: ultimate
ssb  cv25519/8616035453BC2A73
     created: 2025-08-06  expires: 2025-08-06  usage: E
[ultimate] (1). ss-1 <ss-1@ss.ss>

gpg> addkey
Please select what kind of key you want:
   (3) DSA (sign only)
   (4) RSA (sign only)
   (5) Elgamal (encrypt only)
   (6) RSA (encrypt only)
   (7) DSA (set your own capabilities)
   (8) RSA (set your own capabilities)
  (10) ECC (sign only)
  (11) ECC (set your own capabilities)
  (12) ECC (encrypt only)
  (13) Existing key
  (14) Existing key from card
Your selection? 12
Please select which elliptic curve you want:
   (1) Curve 25519 *default*
   (2) Curve 448
   (3) NIST P-256
   (4) NIST P-384
   (5) NIST P-521
   (6) Brainpool P-256
   (7) Brainpool P-384
   (8) Brainpool P-512
   (9) secp256k1
Your selection?
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 20250806T162300
Key expires at Wed Aug  6 21:53:00 2025 IST
Is this correct? (y/N) y
Really create? (y/N) y
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.

sec  ed25519/814378F974576A86
     created: 2025-08-05  expires: 2025-11-03  usage: C
     trust: ultimate      validity: ultimate
ssb  cv25519/8616035453BC2A73
     created: 2025-08-06  expires: 2025-08-06  usage: E
ssb  cv25519/4A8508EDD0FB0D7F
     created: 2025-08-06  expires: 2025-08-06  usage: E
[ultimate] (1). ss-1 <ss-1@ss.ss>

gpg> key 1

sec  ed25519/814378F974576A86
     created: 2025-08-05  expires: 2025-11-03  usage: C
     trust: ultimate      validity: ultimate
ssb* cv25519/8616035453BC2A73
     created: 2025-08-06  expires: 2025-08-06  usage: E
ssb  cv25519/4A8508EDD0FB0D7F
     created: 2025-08-06  expires: 2025-08-06  usage: E
[ultimate] (1). ss-1 <ss-1@ss.ss>

gpg> delkey
Do you really want to delete this key? (y/N) y

sec  ed25519/814378F974576A86
     created: 2025-08-05  expires: 2025-11-03  usage: C
     trust: ultimate      validity: ultimate
ssb  cv25519/4A8508EDD0FB0D7F
     created: 2025-08-06  expires: 2025-08-06  usage: E
[ultimate] (1). ss-1 <ss-1@ss.ss>

gpg> save
```

Now, check decryption, it fails as the original intended recipient subkey does not exist in the primary key `8EAA278178F0756549458FB5814378F974576A86` anymore.

```shell
$ gpg --homedir=base-keys/ --no-default-keyring --keyring=base-keys/base-keyring --decrypt file-to-encrypt.asc
```

<pre style="color : red">
gpg: encrypted with ECDH key, ID 8616035453BC2A73
gpg: public key decryption failed: No secret key
gpg: decryption failed: No secret key
</pre>
